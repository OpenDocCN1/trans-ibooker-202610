Running in PHP 8, on the other hand, error_reporting() returns the value of the last error—in this case, 4437. Also, of course, the if () expression fails, causing no additional output. Here is the result of the same code running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch03 ] #
php php8_at_silencer_err_rep.php
Error Reporting : 4437
Error Number   : 256
Error String   : We Be Bad
```

This concludes consideration of the differences in @ operator usage in PHP 8.

> Tip
> 
> **Best practice:** Do not use the @ error control operator! The intent of the @ operator is to suppress the display of an error message, but you need to consider why this error message is appearing in the first place. By using the @ operator, you are only avoiding providing a solution to a problem!

# Summary

In this chapter, you received an overview of major changes in error handling in PHP 8. You were also given examples of situations where error conditions might arise, and now have an idea of how to properly manage errors in PHP 8. You now have a solid path toward refactoring code that under PHP 8 now produces errors. If your code could potentially lead to any of the conditions described where former Warnings are now Errors, you risk having your code break.

In a like manner, although the second set of error conditions described only produced Notices in the past, these same conditions now cause a Warning. The new set of Warnings gives you a chance to adjust faulty code and prevent having your application devolve into a seriously unstable condition.

Finally, you learned how use of the @ operator is strongly discouraged. In PHP 8, this syntax will no longer mask fatal errors. In the next chapter, you will learn how to create C-language structures and make direct calls to C-language functions in PHP 8.