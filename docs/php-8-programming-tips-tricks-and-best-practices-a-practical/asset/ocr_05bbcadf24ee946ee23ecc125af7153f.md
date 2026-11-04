As you can see from the preceding output, the number 2 is replaced by the number 3. In PHP 8, however, the number 50 is typecast into a string. As this source string doesn't contain the number 50, no replacements are made, as we can see here:

```
root@php8_tips_php8 [ /repo/ch07 ]#
php php7_mb_string_ereg_replace.php
Original: สุนัขจิ้งจอกสีน้ำตาล 2 ตัวกระโดดข้ามรัว
Modified: สุนัขจิ้งจอกสีน้ำตาล 2 ตัวกระโดดข้ามรัว
```

The danger here is that if your code relies upon this silent interpretation process, your application might either fail or exhibit inconsistent behavior. You'll also note a lack of Notice or Warning messages. PHP 8 relies upon the developer to supply the correct arguments!

The *best practice*, if you do actually need to use an ASCII code point, is to use `mb_chr()` to produce the desired search string. The modified code example might look like this:

```
$mod1 = mb_ereg_replace(mb_chr(50), '3', $str);
```

You now have an idea about what changed in the `mbstring` extension. Without this information, you could easily end up writing faulty code. Developers who are unaware of this information might end up making mistakes in PHP 8, such as assuming the `mbstring` aliases are still in place. Such mistaken understanding can easily cause hours of lost time tracking down errors in program code following a PHP 8 migration.

It's now time to have a look at another extension with major changes: the GD extension.

# Dealing with changes to the GD extension

The **GD extension** is an image-manipulation extension that leverages the GD library. GD originally stood for **GIF Draw**. Oddly, the GD library had to withdraw support for the **Graphics Interchange Format (GIF)** after Unisys revoked the open source license for the compression technology used when generating GIFs. After 2004, however, the Unisys patent on this technology expired and GIF support was restored. As it stands today, the PHP GD extension offers support for the **Joint Photographic Experts Group (JPEG or JPG)**, **Portable Network Graphic (PNG)**, **GIF**, **X BitMap (XBM)**, **X PixMap (XPM)**, **Wireless Bitmap (WBMP)**, **WebP**, and **Bitmap (BMP)** formats.