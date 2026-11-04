Starting with PHP 5, along with a new and much more robust OOP implementation, magic methods were introduced. One of these methods is __construct(), specifically reserved for class construction, designed to replace the older usage. Using a method with the same name as the class as a constructor was supported through the remaining versions of PHP 5, and all the way through all versions of PHP 7 as well.

In PHP 8, support for a class constructor method with the same name as the class itself has been removed. If a __construct() method is also defined, you will have no problem: __construct() takes precedence as a class constructor. If there is no __construct() method, and you detect a method with the same name as the class(), you have the potential for failure. Please bear in mind that both method and class names are case-insensitive!

Have a look at the following example. It works in PHP 7 but not in PHP 8:

+   1. First, we define a Text class with a class constructor method of the same name. The constructor method creates an SplFileObject instance based upon the supplied filename:
+   2. We then add three lines of procedural code to exercise the class, supplying the filename of a file containing the Gettysburg Address:

```
// /repo/ch05/php8_oop_bc_break_construct.php
class Text {
    public $fh = '';
    public const ERROR_FN = 'ERROR: file not found';
    public function text(string $fn) {
        if (!file_exists($fn))
            throw new Exception(self::ERROR_FN);
        $this->fh = new SplFileObject($fn, 'r');
    }
    public function getText() {
        return $this->fh->fpassthru();
    }
}
```

```
$fn   = __DIR__ . '../sample_data/gettysburg.txt';
$text = new Text($fn);
echo $text->getText();
```