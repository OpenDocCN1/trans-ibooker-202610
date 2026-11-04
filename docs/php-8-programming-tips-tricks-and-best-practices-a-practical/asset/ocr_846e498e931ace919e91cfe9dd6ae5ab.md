# Error-level defaults

It's useful to note that the error-level defaults assigned to the php.ini file error_reporting directive have been updated in PHP 8. In PHP 7, the default error_reporting level was as follows:

```
error_reporting=E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED
```

In PHP 8, the new level is much simpler, as you can see here:

```
error_reporting=E_ALL
```

It's also worth noting that the php.ini file setting display_startup_errors is now enabled by default. This might be an issue for production servers, as your website might start to unintentionally reveal error information upon PHP startup.

The key takeaway from this section is that in the past, PHP has allowed you to get away with certain bad practices by only issuing Notices or Warnings. As you've learned in this section, however, the danger in not addressing the issues behind the Warning or Notice generation lies in the actions PHP silently takes on your behalf. Not relying upon PHP to make decisions on your behalf leads to fewer hidden logic errors. Following good coding practices, such as that of assigning defaults to all variables before they are used, helps you to avoid such errors. Let's now have a closer look at error situations where Warnings have been promoted to Errors in PHP 8.

# Dealing with warnings that are now errors

In this section, we look at upgraded PHP 8 error handling pertaining to objects, arrays, and strings. We also examine situations where, in the past, PHP issued a Warning but where PHP 8 now throws an Error. It is critical that you become aware of any of the potential error situations addressed in this section. The reason is simple: if you fail to address the situations described in this section, when your server is upgraded to PHP 8 your code will break.

Developers are often pressed for time. It could be that there's a massive queue of new features or other changes that must be made. In other cases, resources have been pulled away to other projects, meaning fewer developers are available to perform maintenance. Warnings are often ignored because the application continues to run, so many developers simply turn off the error display and hope for the best.