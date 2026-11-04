
The source code for this chapter is located here:

```
https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices/tree/main/ch07
```

We can now begin our discussion by examining the overall trend in PHP 8 toward objects rather than resources.

## Understanding the shift from resources to objects

The PHP language has always had an uneasy relationship with resources. Resources represent a connection to an external system such as a file handle or a connection to a remote web service using the client URL (cURL) extension. One big problem with resources, however, is that they defy attempts at data typing. There's no way to distinguish a file handle from a cURL connection—they're both identified as resources.

In PHP 8, a major effort has taken place to move away from resources and to replace them with objects. One of the earliest examples of this trend prior to PHP 8 is the PDO class. When you create a PDO instance, it automatically creates a database connection. Starting with PHP 8, many functions that previously produced a resource now produce an object instance instead. Let's start our discussion by having a look at extension functions that now produce objects rather than resources.

## PHP 8 extension resource-to-object migration

It's important for you to be aware of which functions in PHP 8 now produce objects instead of resources. The good news is that the extension functions have also been rewritten to accommodate an object as an argument rather than a resource. The bad news is that there is a potential backward-compatible code break where you initialize the resource (now object) and test for success using the is_resource() function.

The following table summarizes the functions that formerly returned resources but now return object instances:

| Extension | Function | Returns |
|---|---|---|
| Core | socket_create() <br> socket_create_listen() <br> socket_accept() <br> socket_import_stream() <br> socket_addrinfo_connect() <br> socket_addrinfo_bind() <br> socket_wsprotocol_info_import() <br> socket_addrinfo_lookup() | Socket <br> Socket <br> Socket <br> Socket <br> Socket <br> Socket <br> Socket <br> array of AddressInfo |
| cURL | curl_init() <br> curl_multi_init() <br> curl_share_init() | CurlHandle <br> CurlMultiHandle <br> CurlShareHandle |
| Enchant | enchant_broker_init() <br> enchant_broker_request_dict() <br> enchant_broker_request_pwl_dict() | EnchantBroker <br> EnchantDictionary <br> EnchantDictionary |
| GD | imagecreate*() | GdImage |
| OpenSSL | openssl_x509_read() <br> openssl_csr_sign() <br> openssl_csr_new() <br> openssl_pkey_new() | OpenSSLCertificate <br> OpenSSLCertificate <br> OpenSSLCertificateSigningRequest <br> OpenSSLAsymmetricKey |
| Semaphore | msg_get_queue() <br> sem_get() <br> shm_attach() | SysvMessageQueue <br> SysvSemaphore <br> SysvSharedMemory |
| Shared Memory | shmop_open() | Shmop |
| XML Parser | xml_parser_create() <br> xml_parser_create_ns() | XMLParser <br> XMLParser |
| Zlib | inflate_init() <br> deflate_init() | InflateContext <br> DeflateContext |

Table 7.1 – PHP 8 resource-to-object migration

Table 7.1 serves as a valuable guide to functions that now produce objects rather than resources. Consult this table before you migrate any existing applications to PHP 8. The next section gives you a detailed look at a potential backward-compatible code break and guidelines on how to adjust problematic code, before moving on to the benefits.

## Potential code break involving is_resource()

A problem you might face is that code written prior to PHP 8 assumes the functions listed in Table 7.1 return a resource. Accordingly, clever developers were in the habit of using is_resource() as a test to see if a connection was successfully established.

Although this was an extremely sensible way to check, this technique now introduces a backward-compatible code break after a PHP 8 upgrade. The following example demonstrates this issue.

In this code example, a cURL connection is initialized for an external website. The next few lines test for success using the is_resource() function:

```php
// ///repo/ch07/php7_ext_is_resource.php
$url = 'https://unlikelysource.com/';
$ch  = curl_init($url);
if (is_resource($ch))
    echo "Connection Established\n";
else
    throw new Exception('Unable to establish connection');
```

The following output from PHP 7 shows success:

```
root@php8_tips_php7 [ /repo/ch07 ]#
php php7_ext_is_resource.php
Connection Established
```

The output of the same code running in PHP 8 is not successful, as we can see here:

```
root@php8_tips_php8 [ /repo/ch07 ]#
php php7_ext_is_resource.php
PHP Fatal error:  Uncaught Exception: Unable to establish connection in /repo/ch07/php7_ext_is_resource.php:9
```

The output from PHP 8 is deceptive in that a connection has been established! Because the program code is checking to see if the cURL handle is a resource, however, the code throws an Exception error. The reason for the failure is because a CurlHandle instance is returned rather than a resource.

In this situation, you can avoid a code break and have the code run successfully in both PHP 8 and any earlier PHP version by substituting !empty() (not empty) in place of is_resource(), as shown here:

```php
// ///repo/ch07/php8_ext_is_resource.php
$url = 'https://unlikelysource.com/';
$ch  = curl_init($url);
if (!empty($ch))
    echo "Connection Established\n";
else
    throw new Exception('Unable to establish connection');
var_dump($ch);
```

Here is the output of the code example running in PHP 7:

```
root@php8_tips php7 [ /repo/ch07 ]#
php php8_ext_is_resource.php
Connection Established
/repo/ch07/php8_ext_is_resource.php:11:
resource(4) of type (curl)
```

Here is the same code example running in PHP 8:

```
root@php8_tips php8 [ /repo/ch07 ]#
php php8_ext_is_resource.php
Connection Established
object(CurlHandle)#1 (0) {}
```

As you can see from both outputs, the code runs successfully: in PHP 7, $ch is a resource. In PHP 8, $ch is a CurlHandle instance. Now that you understand the potential issue regarding is_resource(), let's have a look at the advantages that stem from this change.

## Advantages of objects over resources

Prior to PHP 8, there was no way to provide a data type when passing a resource into or returning a resource out of a function or method. A clear advantage in producing objects rather than resources is that you can take advantage of object type hints.

To illustrate this advantage, imagine a set of HyperText Transfer Protocol (HTTP) client classes that implement a strategy software design pattern. One strategy involves using the cURL extension to send a message. Another strategy uses PHP streams, as follows:

- 1. We start by defining an Http/Request class. The class constructor parses the given URL into its component parts, as illustrated in the following code snippet:

```php
// /repo/src/Http/Request.php
namespace Http;
class Request {
    public $url      = '';
    public $method   = 'GET';
    // not all properties shown
    public $query    = '';
    public function __construct(string $url) {
$result = [];
$parsed = parse_url($url);
$vars   = array_keys(get_object_vars($this));
foreach ($vars as $name)
    $this->$name = $parsed[$name] ?? '';
    if (!empty($this->query))
        parse_str($this->query, $result);
    $this->query = $result;
    $this->url   = $url;
}
}
```

- 2. Next, we define a CurlStrategy class that uses the cURL extension to send a message. Note that the __construct() method uses constructor-argument promotion. You might also note that we provide a CurlHandle data type for the $handle argument. This is a tremendous advantage only available in PHP 8, and it ensures that any program creating an instance of this strategy class must provide the correct resource data type. The code is illustrated in the following snippet:

```php
// /repo/src/Http/Client/CurlStrategy.php
namespace Http\Client;
use CurlHandle;
use Http\Request;
class CurlStrategy {
    public function __construct(
        public CurlHandle $handle) {}
```

- 3. We then define the actual logic used to send the message, as follows:

```php
public function send(Request $request) {
    // not all code is shown
    curl_setopt($this->handle,
        CURLOPT_URL, $request->url);
    if (strtolower($request->method) === 'post') {
        $opts = [CURLOPT_POST => 1,
            CURLOPT_POSTFIELDS =>
                http_build_query($request->query)];
        curl_setopt_array($this->handle, $opts);
```
