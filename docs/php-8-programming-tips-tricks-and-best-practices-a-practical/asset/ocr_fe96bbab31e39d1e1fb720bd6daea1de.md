Predictably, the output from PHP 8 produces a Warning, as seen here:

```
PHP Warning: Uninitialized string offset 27 in /repo/ch03/php8_warn_un_init_offset.php on line 5
```

All of the examples in this section confirm the general trend in PHP 8 toward enforcing best coding practices.

> Tip
> 
> For more information on promoted Notices and Warnings, have a look at this article: https://wiki.php.net/rfc/engine_warnings.

Now, we turn our attention to the (infamous) @ warning suppressor.

# Handling the @ error control operator

For years and years, many PHP developers have used the @ error control operator to mask errors. This was especially true when using unreliable PHP libraries with badly written code. Unfortunately, the net effect of this usage only serves to propagate bad code!

Many PHP developers are exercising wishful thinking, believing that when they use the @ operator to prevent errors from being displayed, this makes it seem as if the problem has magically gone away! Trust me when I say this: it hasn’t! In this section, we first examine traditional use of the @ operator, after which we examine @ operator changes in PHP 8.

> Tip
> 
> For more information on traditional @ operator syntax and usage, have a look at this documentation reference page: https://www.php.net/manual/en/language.operators.errorcontrol.php.

# @ operator usage

Before presenting a code example, once again it’s extremely important to emphasize that we are not promoting the usage of this mechanism! On the contrary—you should avoid this usage in every case. If an error message appears, the best solution is to fix the error, not to silence it!