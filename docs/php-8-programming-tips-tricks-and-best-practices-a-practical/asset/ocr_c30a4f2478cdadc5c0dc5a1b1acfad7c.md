# Avoiding problems with the updated mbstring extension

The mbstring extension was first introduced in PHP 4 and has been an active part of the language ever since. The original purpose of this extension was to provide support for the various Japanese character-encoding systems. Since that time, support for a wide variety of other encodings has been added—most notably, support for encodings based upon **Universal Coded Character Set 2 (UCS-2)**, **UCS-4**, **Unicode Transformation Format 8 (UTF-8)**, **UTF-16**, **UTF-32**, **Shift Japanese Industrial Standards (SJIS)**, and **International Organization for Standardization 8859 (ISO-8859)**, among others.

If you aren't sure which encodings are supported on your server, just run the `mb_list_encodings()` command, as follows (partial output shown):

```
root@php8_tips_php7 [ /repo/ch07 ]#
php -r "var_dump(mb_list_encodings());"
Command line code:1:
array(87) {
    ... only selected output is shown ...
    [14] => string(7) "UCS-4BE"
    [16] => string(5) "UCS-2"
    [19] => string(6) "UTF-32"
    [22] => string(6) "UTF-16"
    [25] => string(5) "UTF-8"
    [26] => string(5) "UTF-7"
    [27] => string(9) "UTF7-IMAP"
    [28] => string(5) "ASCII"
    [29] => string(6) "EUC-JP"
    [30] => string(4) "SJIS"
    [31] => string(9) "eucJP-win"
    [32] => string(11) "EUC-JP-2004"
    [76] => string(6) "KOI8-R"
    [78] => string(9) "ArmSCII-8"
    [79] => string(5) "CP850"
    [80] => string(6) "JIS-ms"
    [81] => string(16) "ISO-2022-JP-2004"
    [86] => string(7) "CP50222"
}
```