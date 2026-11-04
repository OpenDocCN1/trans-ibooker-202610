
## Developing a BC break scan class

The BreakScan class is oriented toward a single file. In this class, we define methods that utilize the various break scan configuration just covered. If we need to scan multiple files, the calling program produces a list of files and passes them to BreakScan one at a time.

The BreakScan class can be broken down into two main parts: methods that define infrastructure, and methods that define how to conduct given scans. The latter is primarily dictated by the structure of the configuration file. For each configuration file section, we'll need a BreakScan class method.

Let's have a look at the infrastructural methods first.

## Defining BreakScan class infrastructural methods

In this section, we have a look at the initial part of the BreakScan class. We also cover methods that perform infrastructure-related activities:

- First, we set up the class infrastructure, placing it in the /repo/src/Php8/Migration directory:

```
// /repo/src/Php8/Migration/BreakScan.php
declare(strict_types=1);
namespace Php8\Migration;
use InvalidArgumentException;
use UnexpectedValueException;
class BreakScan {
```

- Next, we define a set of class constants to render messages indicating the nature of any given post-scan failure:

```
const ERR_MAGIC_SIGNATURE = 'WARNING: magic method '
    . 'signature for %s does not appear to match '
    . 'required signature';
const ERR_NAMESPACE = 'WARNING: namespaces can no '
    . 'longer contain spaces in PHP 8.';
const ERR_REMOVED = 'WARNING: the following function'
    . 'has been removed: %s. Use this instead: %s';
// not all constants are shown
```

- We also define a set of constants that represent configuration array keys. We do this to maintain consistency between key definitions in the configuration file and calling program (discussed later):

```php
const KEY_REMOVED = 'removed';
const KEY_CALLBACK = 'callbacks';
const KEY_MAGIC = 'magic';
const KEY_RESOURCE = 'resource';
```

- We then initialize key properties, representing the configuration, the contents of the file to be scanned, and any messages:

```php
public $config = [];
public $contents = '';
public $messages = [];
```

- The __construct() method accepts our break scan configuration file as an argument, and cycles through all of the keys to ensure they exist:

```php
public function __construct(array $config) {
    $this->config = $config;
    $required = [self::KEY_CALLBACK,
        self::KEY_REMOVED,
        self::KEY_MAGIC,
        self::KEY_RESOURCE];
    foreach ($required as $key) {
        if (!isset($this->config[$key])) {
            $message = sprintf(
                self::ERR_MISSING_KEY, $key);
            throw new Exception($message);
        }
    }
}
```

- We then define a method that reads in the contents of the file to be scanned. Note that we remove carriage returns ("\r") and linefeeds ("\n") in order to make scanning via regular expression easier to process:

```php
public function getFileContents(string $fn) {
    if (!file_exists($fn)) {
        throw new Exception(
            sprintf(self::ERR_FILE_NOT_FOUND, $fn));
    }
    $this->contents = file_get_contents($fn);
    $this->contents = str_replace(["\r","\n"],
        ['',' '], $this->contents);
    return $this->contents;
}
```

- Some of the callbacks need a way to extract just the class name, or just the namespace. For that purpose, we define the static getKeyValue() method:

```
public static function getKeyValue(
    string $contents, string $key, string $end) {
    $pos = strpos($contents, $key);
    $end = strpos($contents, $end,
        $pos + strlen($key) + 1);
    return trim(substr($contents,
        $pos + strlen($key),
        $end - $pos - strlen($key)));
}
```

This method looks for the keyword (for example, class). It then finds whatever follows the keyword up to the delimiter (for example, '; '). So, if you want to get the class name, you would execute the following: $name = BreakScan::getKeyValue($contents,'class',';').

- We also need a way to retrieve and reset $this->messages. Here are the two methods to do that:

```
public function clearMessages() : void {
    $this->messages = [];
}
public function getMessages(bool $clear = FALSE) {
    $messages = $this->messages;
    if ($clear) $this->clearMessages();
    return $messages;
}
```

- We then define a method that runs all scans (covered in the next section). This method also collects the number of potential BC breaks detected and reports back the total:

```php
public function runAllScans() : int {
    $found = 0;
    $found += $this->scanRemovedFunctions();
    $found += $this->scanIsResource();
    $found += $this->scanMagicSignatures();
    $found += $this->scanFromCallbacks();
    return $found;
}
```

Now that you have an idea of how the basic BreakScan class infrastructure might appear, let's have a look at the individual scan methods.

## Examining individual scan methods

The four individual scan methods correspond directly to the top-level keys in the break scan configuration file. Each method is expected to accumulate messages about potential BC breaks in $this->messages. In addition, each method is expected to return an integer representing the total number of potential BC breaks detected.

Let's now examine these methods in order:

- The first method we examine is scanRemovedFunctions(). In this method, we search for the function name followed either directly by an open parenthesis, ' (', or by a space and open parenthesis, ' ( '. If the function is found, we increment $found, and add the appropriate warning and suggested replacement to $this->messages. If no potential breaks are found, we add a success message and return 0:

```php
public function scanRemovedFunctions() : int {
    $found = 0;
    $config = $this->config[self::KEY_REMOVED];
    foreach ($config as $func => $replace) {
        $search1 = ' ' . $func . '(';
        $search2 = ' ' . $func . ' (';
        if (
            strpos($this->contents, $search1) !== FALSE
            ||
            strpos($this->contents, $search2) !== FALSE)
        {
            $this->messages[] = sprintf(
                self::ERR_REMOVED, $func, $replace);
            $found++;
        }
    }
    if ($found === 0)
        $this->messages[] = sprintf(
            self::OK_PASSED, __FUNCTION__);
    return $found;
}
```

The main problem with this approach is that if the function is not preceded by a space, its use would not be detected. However, if we do not include the leading space in the search, we could end up with a false positive. For example, without the leading space, every single instance of foreach() would trigger a warning by the break scanner when looking for each()!

- Next we have a look at a method that scans for is_resource() usage. If a reference is located, this method runs through the list of functions that no longer produce a resource. If both is_resource() and one of these methods is located, a potential BC break is flagged:

```php
public function scanIsResource() : int {
    $found = 0;
    $search = 'is_resource';
    if (strpos($this->contents, $search) === FALSE)
        return 0;
    $config = $this->config[self::KEY_RESOURCE];
    foreach ($config as $func) {
        if ((strpos($this->contents, $func) !== FALSE)) {
            $this->messages[] =
                sprintf(self::ERR_IS_RESOURCE, $func);
            $found++;
        }
    }
    if ($found === 0)
        $this->messages[] =
            sprintf(self::OK_PASSED, __FUNCTION__);
    return $found;
}
```

- We then have a look at what’s required to go through our list of callbacks. As you recall, we need to employ callbacks in situations where a simple strpos() is insufficient. Accordingly, we first collect all the callback subkeys and loop through each one in turn. If there is no bottom-level key callback, we throw an Exception. Otherwise, we run the callback, supplying $this->contents as an argument. If any potential BC breaks are found, we add the appropriate error message, and increment $found:

```
public function scanFromCallbacks() {
    $found = 0;
    $list = array_keys($this->config[self::KEY_CALLBACK]);
    foreach ($list as $key) {
        $config = $this->config[self::KEY_CALLBACK][$key] ?? NULL;
        if (empty($config['callback']) || !is_callable($config['callback'])) {
            $message = sprintf(self::ERR_INVALID_KEY, self::KEY_CALLBACK . ' => ' . $key . ' => callback');
            throw new Exception($message);
        }
        if ($config['callback']($this->contents)) {
            $this->messages[] = $config['msg'];
            $found++;
        }
    }
    return $found;
}
```
