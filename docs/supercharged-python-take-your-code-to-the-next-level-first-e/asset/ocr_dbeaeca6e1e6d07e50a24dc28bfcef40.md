supposed to be “infinite” or, rather, that the limits on integers
are astronomical. That’s correct, but you give up this flexibility
when you deal with fixed-length structures.
One of the limitations of the **array** package and its **array**
type is that it supports one-dimensional arrays only.

# 12.3 DOWNLOADING AND
IMPORTING “NUMPY”

To try out or use any of the code in the remainder of this
chapter, you’ll need to download **numpy** if you haven’t already.
If you’re working in IDLE or writing a Python script and if you
attempt to import **numpy**, Python will raise a
**ModuleNotFoundError** exception if the **numpy** package is not
present.
If that happens, you need to download it. The easiest way to
do that is to use the **pip** utility, assuming that it’s present.
One of the benefits of **pip** is that it goes out to the standard
storage locations for publishing packages on the Internet, finds
the requested software, and downloads it for you. So, assuming
**pip** is present on your system, all you need to do is start a DOS
box or Terminal application, and then type the following:

```
pip download numpy
```

If you’re working with a Macintosh system and you have
Python 3 installed, you may instead need to work with the
command **pip3**.