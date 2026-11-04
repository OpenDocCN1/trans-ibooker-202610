
The examples that follow are executed in a PHP 8.1 Docker container. Be sure to bring down the existing containers as follows, from a command prompt on your local computer on a Windows computer: C:\path\to\repo\init down. For Linux or Mac, from a terminal window: /path/to/repo/init.sh down. To bring up the PHP 8.1 container from a Windows computer: C:\path\to\repo\ch12\init up. From a Linux or Mac terminal window: /path/to/repo/ch12/init.sh up.

The examples that follow are executed in a PHP 8.1 Docker container. Be sure to bring down the existing containers as follows from a command prompt on your local computer on a Windows computer: C:\path\to\repo\init down

For Linux or Mac, from a terminal window:

```
/path/to/repo/init.sh down
```

To bring up the PHP 8.1 container from a Windows computer:

```
C:\path\to\repo\ch12\init up
```

From a Linux or Mac terminal window:

```
/path/to/repo/ch12/init.sh up
```

We now look at the source code for the core API program itself, as follows:

- 1. First, we define a Chat\Message\Pipe class, identifying all of the external classes we need to use, like this:

```php
// /repo/src/Chat/Message/Api.php;
namespace Chat\Message;
use Chat\Handler\ {GetHandler, PostHandler,
    NextHandler,GetAllNamesHandler,DeleteHandler};
use Chat\Middleware\ {Access,Validate,ValidatePost};
use Chat\Message\Render;
use Psr\Http\Message\ServerRequestInterface;
class Pipe {
```
```

- 2. We then define an exec() static method that invokes a set of PHP Standard Recommendation 15 (PSR-15)-compliant handlers. We also invoke the first stage of the pipe by calling the process method of the Chat\Middleware\Access middleware class. The return value of NextHandler is ignored:

```php
public static function exec(
    ServerRequestInterface $request) {
    $params = $request->getQueryParams();
```
```

```
$method = strtolower($request->getMethod());
$dontcare = (new Access())
    ->process($request, new NextHandler());
```

3. Still in the same method, we use a match() construct to check for HTTP GET, POST, and DELETE method calls. If the method is POST, we use the Chat\Middleware\ValidatePost validation middleware class to validate the POST arguments. If validation succeeds, the sanitized data is then passed to Chat\Handler\PostHandler. If the HTTP method is DELETE, we directly call Chat\Handler\DeleteHandler:

```
$response = match ($method) {
    'post' => (new ValidatePost())
        ->process($request, new PostHandler()),
    'delete' => (new DeleteHandler())
        ->handle($request),
```

4. If the HTTP method is GET, we first check to see if the all parameter is set. If so, we invoke Chat\Handler\GetAllNamesHandler. Otherwise, the default clause passes data through Chat\Middleware\Validate. If validation succeeds, the sanitized data is passed to Chat\Handler\GetHandler:

```
'get' => (!empty($params['all'])
    ? (new GetAllNamesHandler())->handle($request)
    : (new Validate())->process($request,
        new GetHandler())),
    default => (new Validate())
        ->process($request, new GetHandler()});
    return Render::output($request, $response);
}
```

5. The core API class can then be called using a short conventional program, as shown here. In this calling program, we build a PSR-7 compliant Psr\Http\Message\ServerRequestInterface instance using Laminas\Diactoros\ServerRequestFactory. The request is then passed through the Pipe class and a response is produced:

```
// /repo/ch12/php8_chat_ajax.php
include __DIR__ . '/vendor/autoload.php';
```

```php
use Laminas\Diactoros\ServerRequestFactory;
use Chat\Message\Pipe;
$request = ServerRequestFactory::fromGlobals();
$response = Pipe::exec($request);
echo $response;
```
```

We also created a test program (/repo/ch12/php8_chat_test.php—not shown) that calls the API endpoint a set number of times (the default is 100). On each iteration, the test program posts a random message consisting of a random recipient username, a random date, and a sequential entry from the /repo/sample_data/geonames.db database. The test program takes two arguments. The first argument is a URL that represents the API. The second (optional) argument represents the number of iterations.

Here are example results running /ch12/php8_chat_test.php from a command shell into the PHP 8.1 Docker container:

```
root@php8_tips_php8_1 [/repo/ch12 ]# php php8_chat_test.php \
http://localhost/ch12/php8_chat_ajax.php 10000  bboyer :
Dubai:AE:2956587 : 2021-01-01 00:00:00
1 fcompton :           Sharjah:AE:1324473 : 2022-02-02 01:01:01
...
998 hrivas : Caloocan City:PH:1500000 : 2023-03-19 09:54:54
999 lpena :             Budta:PH:1273715 : 2021-04-20 10:55:55
From User: dwallace
Elapsed Time: 3.3177478313446
```
```

From the output, take note of the elapsed time. In the next section, using Swoole, we are able to cut this time in half! Before using Swoole, however, it is only fair to incorporate the JIT compiler. We enable JIT using the following command:

```bash
# php /repo/ch10/php8_jit_reset.php on
```
```

In PHP 8.0.0, it's possible you might encounter a few errors and possibly a segmentation fault. In PHP 8.1, however, the API should work as expected with the JIT compiler enabled. However, it's highly doubtful the JIT compiler would improve performance, as the frequent API calls cause the application to wait. Any application with frequent blocking I/O operations is an excellent candidate for the asynchronous programming model. Before we proceed, however, we need to turn off JIT, using the same utility program as before, as follows:

```bash
# php /repo/ch10/php8_jit_reset.php off
```
```

Let's now have a look at how the Swoole extension might be used to improve the performance of this I/O-intensive application.

## Using the Swoole extension to improve application performance

Given that Swoole provides coroutine support, all we really need to do in order to improve the Chat application performance is to rewrite the /repo/ch12/php8_chat_ajax.php calling program, turning it into an API that listens on port 9501 as a Swoole server instance. Here are the steps to rewrite the main API calling program:

- 1. First, we enable autoloading and identify the external classes needed:

```
// /repo/ch12/php8_chat_swoole.php
include __DIR__ . '/vendor/autoload.php';
use Chat\Message\Pipe;
use Chat\Http\SwooleToPsr7;
use Swoole\Http\Server;
use Swoole\Http\Request;
use Swoole\Http\Response;
```

- 2. Next, we start a PHP session and create a Swoole\HTTP\Server instance that listens for any IP address on port 9501:

```
session_start();
$server = new Swoole\HTTP\Server('0.0.0.0', 9501);
```

- 3. We then invoke the on() method and associate it with the start event. In this case, we make a log entry to identify when the Swoole server started. Other server events are documented here: https://www.swoole.co.uk/docs/modules/swoole-http-server-doc:

```
$server->on("start", function (Server $server) {
    error_log('Swoole http server is started at '
        . 'http://0.0.0.0:9501');
});
```

- 4. Finally, we define a main server event, $server->on('request', function () {}), which handles incoming requests. Here's the code to accomplish this:

```php
$server->on("request", function (
    Request $swoole_request, Response $swoole_response) {
    $request = SwooleToPsr7::
        swooleRequestToServerRequest($swoole_request);
    $swoole_response->header(
        "Content-Type", "text/plain");
    $response = Pipe::exec($request);
    $swoole_response->end($response);
});
$server->start();
```
```

Unfortunately, a Swoole\Http\Request instance passed to the callback associated with the on() method is not PSR-7-compliant! Accordingly, we need to define a Chat\Http\SwooleToPsr7 class and a swooleRequestToServerRequest() method that performs the conversion using a static call. We then set headers on the Swoole\Http\Response instance and return a value from the pipe to complete the circuit.

It's extremely important for you to note that the standard PHP superglobals, such as $_GET and $_POST, do not work as expected from a running Swoole server instance. The main point of entry is the initial program you use to start the Swoole server from the command line. The only incoming request parameter is the actual initial program filename. Any subsequent input must be captured through the Swoole\Http\Request instance that's passed to the on() function.

The documentation found at https://php.net/swoole does not show all of the methods available for the Swoole\HTTP\Request and Swoole\HTTP\Response classes. However, on the Swoole website itself, you can find the relevant documentation, which is also listed here:

- https://www.swoole.co.uk/docs/modules/swoole-http-request
- https://www.swoole.co.uk/docs/modules/swoole-http-response

It's also worth noting that the Swoole\HTTP\Request object properties roughly correspond to the PHP superglobals, as shown here:
