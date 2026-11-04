3. In the body of the __construct() method, assign values to properties, like this:

```
{
    $this->text     = $text;
    $this->fontFile = $fontFile;
    $this->width    = $width;
    $this->height   = $height;
    $this->size     = $size;
    $this->angle    = $angle;
    $this->textX    = $textX;
    $this->textY    = $textY;
    // other code not shown
}
```

As the number of constructor arguments increases, the amount of work you need to do also increases significantly. When constructor property promotion is applied, the amount of code required to do the same as previously shown is reduced to one-third of the original.

Let's now have a look at the same block of code as shown previously, but rewritten using this powerful new PHP 8 feature:

```
// /repo/src/Php8/Image/SingleChar.php
// not all code shown
public function __construct(
    public string $text,
    public string $fontFile,
    public int    $width  = 100,
    public int    $height = 100,
    public int    $size   = self::DEFAULT_TX_SIZE,
    public float  $angle  = self::DEFAULT_TX_ANGLE,
    public int    $textX  = self::DEFAULT_TX_X,
    public int    $textY  = self::DEFAULT_TX_Y)
{ // other code not shown }
```

Amazingly, what took 24 lines of code in PHP 7 and earlier can be collapsed into eight lines of code using this new PHP 8 feature!

You are completely free to include other code in the constructor. In many cases, however, constructor property promotion takes care of everything normally done in the __construct() method, which means you can literally leave it empty ({ }).