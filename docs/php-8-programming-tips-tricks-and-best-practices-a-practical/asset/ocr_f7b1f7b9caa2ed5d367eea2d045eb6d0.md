As you can see from the output, the number is stored internally using a period for a decimal separator for en_GB, whereas the comma is used for locales fr_FR and de_DE. However, when the string is converted back to a number, the string is treated as a leading-numeric string if the decimal separator is not a period. In two of the locales, the presence of the comma stops the conversion process. The net effect is that the decimal portion is dropped and precision is lost.

The results when running the same code sample in PHP 8 are shown here:

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_locale_independent.php
Locale          : en_GB
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
Locale          : fr_FR
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
Locale          : de_DE
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
```

In PHP 8, no precision is lost and the number is consistently represented using a period for the decimal separator, regardless of the locale.

Please note that you can still represent a number according to its locale by using the number_format() function, or by using the NumberFormatter class (from the Intl extension). It's interesting to note that the NumberFormatter class stores numbers internally in a locale-independent manner!

> **Tip**
> 
> For more information, have a look at this article: https://wiki.php.net/rfc/locale_independent_float_to_string.
> 
> For more information on international number formatting, refer to the following link:
> https://www.php.net/manual/en/class.numberformatter.php