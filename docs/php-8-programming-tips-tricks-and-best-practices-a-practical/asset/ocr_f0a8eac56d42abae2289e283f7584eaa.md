3. We then loop through each character in the CAPTCHA phrase and create a SingleChar instance. The initial call to writeFill() creates the white background canvas. We also need to call shuffle() to randomize the list of distortion strategies. The process is illustrated in the following code snippet:

```
```php
$images = [];
for ($x = 0; $x < $length; $x++) {
    $char = new SingleChar($phrase[$x], FONT_FILE);
    $char->writeFill();
    shuffle($strategies);
}
```
```

4. We then loop through the strategies and layer distortions upon the original image. This is where the match expression comes into play. Notice that one strategy needs additional lines of code. Because match can only support a single expression, we simply wrap the multiple lines of code into an anonymous function, as follows:

```
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
```