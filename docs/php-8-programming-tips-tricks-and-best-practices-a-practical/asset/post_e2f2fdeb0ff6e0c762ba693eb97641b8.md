
If we run this code example, we'll see that the two lists are identical in both PHP 7 and PHP 8. Here's the PHP 7 output:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_class_normal.php
Array (
    [0] => Php7\Image\Strategy\DotFill
    [1] => Php7\Image\Strategy\LineFill
    [2] => Php7\Image\Strategy\PlainFill
    [3] => Php7\Image\Strategy\RotateText
    [4] => Php7\Image\Strategy\Shadow
)
Array (
    [0] => Php7\Image\Strategy\DotFill
    [1] => Php7\Image\Strategy\LineFill
    [2] => Php7\Image\Strategy\PlainFill
    [3] => Php7\Image\Strategy\RotateText
    [4] => Php7\Image\Strategy\Shadow
)
```

As you can see, the special ::class constant causes class names to be expanded into their full namespace, plus class name, at compile time, causing both lists to contain identical information.

Now, let's have a look at special ::class constant usage in PHP 8.

## Expanded special ::class constant usage

In line with other improvements in syntax uniformity seen in PHP 8, it's now possible to use the special ::class constant on active object instances. Although the effect is the same as using get_class(), it makes sense to use the special ::class constant as part of the general best practice of moving away from procedural and toward OOP.

In this example, the extended ::class syntax is used to determine the type of error that's thrown:

- 1. When an Error or Exception is thrown, it's best practice to make an entry in the error log. In this example, which works in both PHP 7 and PHP 8, the class name of this Error or Exception is included in the log message:

```php
// /repo/ch09/php7_class_and_obj.php
try {
    $pdo = new PDO();
    echo 'No problem';
} catch (Throwable $t) {
    $msg = get_class($t) . ':' . $t->getMessage();
    error_log($msg);
}
```

- 2. In PHP 8, you can achieve the same result by rewriting the example, as follows:

```php
// /repo/ch09/php8_class_and_obj.php
try {
    $pdo = new PDO();
    echo 'No problem';
} catch (Throwable $t) {
    $msg = $t::class . ':' . $t->getMessage();
    error_log($msg);
}
```

As you can see from the second block of code, the syntax is more concise and avoids the use of a procedural function. We must stress, however, that in this example, there is no performance gain.

Now that you are aware of changes in special ::class constant usage, let's have a quick look at commas.

## Taking advantage of trailing commas

PHP has long allowed the use of trailing commas when defining arrays. For example, the syntax shown here is not uncommon:

```
$arr = [1, 2, 3, 4, 5,];
```

However, doing the same thing in a function or method signature is not allowed:

```
function xyz ($fn, $ln, $mid = '',) { /* code */ }
```

Although it's not really such a big deal, it's annoying to be able to add a trailing comma when defining an array, but not be allowed the same liberty when it comes to function or method signatures!

PHP 8 now allows you to use trailing commas in both function and method signatures. The new rule also applies to use () statements associated with anonymous functions.

To illustrate this change, consider the following example. In this example, an anonymous function is defined that renders a full name:

```
// /repo/ch09/php8_trailing_comma.php
$full = function ($fn, $ln, $mid = '',) {
    $mi = ($mid) ? strtoupper($mid[0]) . '. ' : '';
    return $fn . ' ' . $mi . $ln;
};
echo $full('Fred', 'Flintstone', 'John');
```

As you can see, there's a comma following the third argument to the anonymous function. Here is the output from PHP 7:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_trailing_comma.php
PHP Parse error: syntax error, unexpected ')', expecting variable (T_VARIABLE) in /repo/ch09/php8_trailing_comma.php on line 4
```

In PHP 8, the trailing comma is allowed and the expected output appears, as shown here:

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_trailing_comma.php
Fred J. Flintstone
```

Although having a trailing comma in function or method definitions is not necessarily a best practice, it does make PHP 8 consistent in its overall treatment of trailing commas.

Now, let's turn our attention to methods that still exist but no longer have any use.

## Learning about methods that are no longer required

Primarily as a result of the PHP 8 resource-to-object migration, a number of functions and methods are no longer required. They are not deprecated at the time of writing, but these functions no longer serve any practical use.

As an analogy, in versions of PHP before PHP 8, you would use fopen() to open a file handle resource. Once you had finished working on the file, you would normally use fclose() on the file handle resource to close the connection.

Now, let's assume that you are using SplFileObject instead of fopen(). When the work with the file has been completed, you can simply unset the object. This accomplishes the same thing as using fclose(), making fclose() redundant.

The following table summarizes functions that exist, and can still be used, but no longer have any practical value in PHP 8. The ones marked with an asterisk are also deprecated:

| Extension | Functions No Longer Useful |
|---|---|
| cURL | curl_close()
 curl_multi_close()
 curl_share_close() |
| Exif | read_exif_data() |
| GD | imagedestroy() |
| OpenSSL | openssl_x509_free() * |
| Shared Memory | shmop_close() * |
| XMLParser | xml_parser_free() |

Table 9.1 – Functions that are no longer useful

Now that you have an idea of what major method signature and usage changes have been made in PHP 8, let's have a look at the best practices to take into consideration when working with interfaces and traits.

## Working with interfaces and traits

The PHP 8 trait implementation has been expanded in several ways. There are also several new interfaces that can potentially alter how you work with the DOM and DateTime extensions. For the most part, the changes improve the abilities of these two extensions. However, as method signatures have changed in some cases, you may run into a potential code break. Due to this, it's important to pay close attention to the discussion presented in this section to ensure that existing and future PHP code remains functional.

First, let's have a look at the new DOM extension interfaces.

### Discovering new DOM extension interfaces

The *cost of living* economic statistic is issued each year by many world governments. It depicts how much it costs an average citizen to live year by year. As web technology matures, a similar principle has been applied – first to HTML, and now to DOM. The **DOM Living Standard** is maintained by the **Web Hypertext Application Technology Working Group (WHATWG)** (`https://whatwg.org/`).

The reason why this information is important to PHP developers is that in PHP 8, the decision was made to move the PHP DOM extension to the DOM Living Standard. Accordingly, starting with PHP 8, a series of incremental and continuous changes will be applied to this extension in accordance with changes to the living standard.

For the most part, the changes are backward compatible. However, since some method signatures change to maintain adherence to the standard, you may experience code breaks. The most significant change to the DOM extension in PHP 8 is the introduction of two new interfaces. Let's examine these interfaces, and then discuss their impact on PHP development.

### Examining the new DOMParentNode interface

The first of the two new interfaces is **DOMParentNode**. The following classes implement this interface in PHP 8:

- `DOMDocument`
- `DOMElement`
- `DOMDocumentFragment`

Here is the interface definition:

```
interface DOMParentNode {
    public readonly ?DOMElement $firstElementChild;
    public readonly ?DOMElement $lastElementChild;
    public readonly int $childElementCount;
    public function append(
        ...DOMNode|string|null $nodes) : void;
    public function prepend(
        ...DOMNode|string|null $nodes) : void;
}
```

It's important to note that there is no readonly attribute available to PHP developers. However, the interface specification shows the properties as read only because they are internally generated and cannot be changed.

> **Tip**
> There actually was a PHP RFC that was introduced in 2014 that proposed adding a readonly attribute to class properties. This proposal was withdrawn, however, as the same effect can be achieved by defining a constant or simply marking a property private! For more information on this proposal, see https://wiki.php.net/rfc/readonly_properties.

The following table summarizes the properties and methods of the new DOMParentNode interface:

| Method / Property | Description |
|---|---|
| $firstElementChild | Contains either the first DOMElement instance in this object, or NULL. |
| $lastElementChild | Contains either the last DOMElement instance in this object, or NULL. |
| $elementChildCount | Integer value representing the total number of child elements in this object. |
| append() | Appends the node to the end of the current list of child nodes. |
| prepend() | Adds the node to the beginning of the current list of child nodes. |

Table 9.2 – DOMParentNode interface methods and properties
