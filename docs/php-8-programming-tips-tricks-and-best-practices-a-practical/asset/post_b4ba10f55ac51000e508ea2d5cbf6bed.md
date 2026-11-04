
Other than the differences noted, match and switch both allow case aggregation, as well as providing support for a default case.

## switch and match examples

Here is a simple example that renders a currency symbol using switch:

```php
// /repo/ch01/php7_switch.php
function get_symbol($iso) {
    switch ($iso) {
        case 'CNY' :
            $sym = '¥';
            break;
        case 'EUR' :
            $sym = '€';
            break;
        case 'EGP' :
            case 'GBP' :
            $sym = '£';
            break;
        case 'THB' :
            $sym = '฿';
            break;
        default :
            $sym = '$';
    }
    return $sym;
}
$test = ['CNY', 'EGP', 'EUR', 'GBP', 'THB', 'MXD'];
foreach ($test as $iso)
    echo 'The currency symbol for ' . $iso
        . ' is ' . get_symbol($iso) . "\n";
```

When this code is executed, you see the currency symbols for each of the **International Organization for Standardization (ISO)** currency codes in the $test array. The same result as that shown in the preceding code snippet can be obtained in PHP 8, using the following code:

```php
// /repo/ch01/php8_switch.php
function get_symbol($iso) {
    return match ($iso) {
        'EGP', 'GBP' => '£',
        'CNY'        => '¥',
        'EUR'        => '€',
        'THB'        => '฿',
        default      => '$'
    };
}
$test = ['CNY', 'EGP', 'EUR', 'GBP', 'THB', 'MXD'];
foreach ($test as $iso)
    echo 'The currency symbol for ' . $iso
        . ' is ' . get_symbol($iso) . "\n";
```

Both examples produce an identical output, as illustrated here:

```
The currency symbol for CNY is ¥
The currency symbol for EGP is £
The currency symbol for EUR is €
The currency symbol for GBP is £
The currency symbol for THB is ฿
The currency symbol for MXD is $
```

As mentioned previously, both code examples produce a list of currency symbols for the list of ISO currency codes stored in the $test array.

## Complex match example

Returning to our CAPTCHA project, assume that we wish to introduce distortion to make the CAPTCHA characters more difficult to read. To accomplish this goal, we introduce a number of **strategy** classes, each producing a different distortion, as summarized in this table:

| Php8\Image\Strategy\* | Description |
|---|---|
| DotFill ($num) | Fills background with $num dots |
| LineFill ($num) | Fills background with $num lines |
| RotateText ($angle) | Rotates text by $angle |
| Shadow ($offset, $r, $g, $b) | Adds a shadow with $offset pixels apart, with a color determined by $r, $g and $b (red, green, blue) |

Table 1.2 – CAPTCHA distortion strategy classes

After randomizing the list of strategies to be employed, we use a match expression to execute the results, as follows:

- 1. First we define an **autoloader**, import the classes to be used, and list potential strategies to employ, as illustrated in the following code snippet:
- 2. Next, we generate the CAPTCHA phrase, as follows:

```php
// /repo/ch01/php8_single_strategies.php
// not all code is shown
require_once __DIR__ . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php8\Image\SingleChar;
use Php8\Image\Strategy\{LineFill,DotFill,Shadow,RotateText};
$strategies = ['rotate', 'line', 'line',
    'dot', 'dot', 'shadow'];
```

```php
$phrase = strtoupper(bin2hex(random_bytes(NUM_BYTES)));
$length = strlen($phrase);
```

- 3. We then loop through each character in the CAPTCHA phrase and create a SingleChar instance. The initial call to writeFill() creates the white background canvas. We also need to call shuffle() to randomize the list of distortion strategies. The process is illustrated in the following code snippet:

```php
$images = [];
for ($x = 0; $x < $length; $x++) {
    $char = new SingleChar($phrase[$x], FONT_FILE);
    $char->writeFill();
    shuffle($strategies);
}
```

- 4. We then loop through the strategies and layer distortions upon the original image. This is where the match expression comes into play. Notice that one strategy needs additional lines of code. Because match can only support a single expression, we simply wrap the multiple lines of code into an anonymous function, as follows:

```php
foreach ($strategies as $item) {
    $func = match ($item) {
        'rotate' => RotateText::writeText($char),
        'line' => LineFill::writeFill(
            $char, rand(1, 10)),
        'dot' => DotFill::writeFill($char, rand(10, 20)),
        'shadow' => function ($char) {
            $num = rand(1, 8);
            $r   = rand(0x70, 0xEF);
            $g   = rand(0x70, 0xEF);
            $b   = rand(0x70, 0xEF);
            return Shadow::writeText(
                $char, $num, $r, $g, $b);},
        'default' => TRUE
    };
    if (is_callable($func)) $func($char);
}
```

- 5. All that remains to be done is to overlay the image with the actual CAPTCHA phrase by calling writeText() with no arguments. After that, we save the distorted image as a **Portable Network Graphics (PNG)** file for display, as illustrated in the following code snippet:

```php
$char->writeText();
$fn = $x . '_'
    . substr(basename(__FILE__), 0, -4)
    . '.png';
$char->save(IMG_DIR . '/' . $fn);
$images[] = $fn;
}
include __DIR__ . '/captcha_simple.phtml';
```

Here is the result, running the preceding example from a browser that points to the Docker container associated with this book:

![](img/dbf2674d5b31107c344104b5edcb21c5_45_0.png)

Figure 1.1 – Distorted CAPTCHA using match expression

Next, we'll have a look at another really great feature: named arguments.

> **Tip**
> You can see the original proposal for match expressions here:
> https://wiki.php.net/rfc/match_expression_v2

## Understanding named arguments

Named arguments represent a way to avoid confusion when calling functions or methods with a large number of arguments. This not only helps avoid problems with arguments being supplied in an incorrect order, but also helps you to skip arguments with defaults. In this section, you will learn how to apply named arguments to improve the accuracy of your code, reduce confusion during future maintenance cycles, and make your method and function calls more concise. We start by examining the generic syntax required to use named arguments.

## Named argument generic syntax

In order to use named arguments, you need to know the names of the variables used in the function or method signature. You then specify that name, without the dollar sign, followed by a colon and the value to be supplied, as follows:

```
$result = function_name( arg1 : <VALUE>, arg2 : <value> );
```

When the function_name() function is invoked, the values are passed to the arguments corresponding to arg1, arg2, and so on.

## Calling core functions using named arguments

One of the most common reasons to use named arguments is when you call a core PHP function that has a large number of parameters. As an example, here's the function signature for setcookie():

```
setcookie ( string $name [, string $value = ""
    [, int $expires = 0 [, string $path = ""
    [, string $domain = "" [, bool $secure = FALSE
    [, bool $httponly = FALSE ]]]]] ) : bool
```

Let's say that all you really wanted to set were the name, value, and httponly arguments. Before PHP 8, you would have had to look up the default values and supply them, in order, until you got to the one you wished to override. In the following case, we wish to set httponly to TRUE:

```
setcookie('test',1,0,0,'','',FALSE,TRUE);
```

Using named arguments, the equivalent in PHP 8 would be as follows:

```
setcookie('test',1,httponly: TRUE);
```

Note that we do not need to name the first two parameters as they are supplied in order.

> **Tip**
> In PHP extensions, named arguments do not always match the names of variables you see in the PHP documentation for function or method signatures. As an example, the function `imagefttext()` shows a variable `$font_filename` in its function signature. If you scroll down a bit further, however, you'll see in the *Parameters* section, that the named parameter is `fontfile`.
> 
> If you encounter a fatal Error: Unknown named parameter `$NAMED_PARAM`. Always use the name as listed in the *Parameters* section of the documentation rather than the name of the variable in the function or method signature.

## Order independence and documentation

Another use for named arguments is to provide **order independence**. In addition, for certain core PHP functions, the sheer number of parameters presents a documentation nightmare.

As an example, have a look here at the function signature for `imagefttext()` (note that this function is central to the chapter project of producing a secure CAPTCHA image):

```php
imagefttext ( object $image , float $size , float $angle ,
    int $x , int $y , int $color , string $fontfile ,
    string $text [, array $extrainfo ] ) : array
```

As you can imagine, trying to remember the names and order of these parameters when reviewing your work 6 months later might be problematic.

> **Important note**
> In PHP 8, the image creation functions (for example, `imagecreate()`) now return a `GdImage` object instance instead of a resource. All image functions in the GD extension have been rewritten to accommodate this change. There's no need to rewrite your code!
