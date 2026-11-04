Note that we do not need to name the first two parameters as they are supplied in order.

> **Tip**
> In PHP extensions, named arguments do not always match the names of variables you see in the PHP documentation for function or method signatures. As an example, the function `imagefttext()` shows a variable `$font_filename` in its function signature. If you scroll down a bit further, however, you'll see in the *Parameters* section, that the named parameter is `fontfile`.
> 
> If you encounter a fatal Error: Unknown named parameter `$NAMED_PARAM`. Always use the name as listed in the *Parameters* section of the documentation rather than the name of the variable in the function or method signature.

# Order independence and documentation

Another use for named arguments is to provide **order independence**. In addition, for certain core PHP functions, the sheer number of parameters presents a documentation nightmare.

As an example, have a look here at the function signature for `imagefttext()` (note that this function is central to the chapter project of producing a secure CAPTCHA image):

```
```php
imagefttext ( object $image , float $size , float $angle ,
    int $x , int $y , int $color , string $fontfile ,
    string $text [, array $extrainfo ] ) : array
```
```

As you can imagine, trying to remember the names and order of these parameters when reviewing your work 6 months later might be problematic.

> **Important note**
> In PHP 8, the image creation functions (for example, `imagecreate()`) now return a `GdImage` object instance instead of a resource. All image functions in the GD extension have been rewritten to accommodate this change. There's no need to rewrite your code!