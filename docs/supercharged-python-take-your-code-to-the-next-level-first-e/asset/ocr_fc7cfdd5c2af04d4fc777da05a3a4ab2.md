representation within Python code.

> Note
> 
> Where !s or !r would normally appear, you can also use !a, which is similar to !s
> but returns an ASCII-only string.

# 5.8 THE “SPEC” FIELD OF THE
“FORMAT” FUNCTION AND
METHOD

This section and all its subsections apply to both the global
format function and the format method. However, most of
the examples in the remainder of the chapter assume the use of
the format method, which is why they show spec in the
context of a print field, { }, and a colon ( : ).

The syntax of the spec, the format specifier, is the most
complex part of format method grammar. Each part is
optional, but if used, it must observe the order shown. (The
square brackets indicate that each of these items is optional.)

![](bbox=[0.149, 0.688, 0.257, 0.771])

Click here to view code image

```
[ [ fill ]align ][ sign ][ # ][ 0 ][ width ][ , ][ .prec ][ type ]
```