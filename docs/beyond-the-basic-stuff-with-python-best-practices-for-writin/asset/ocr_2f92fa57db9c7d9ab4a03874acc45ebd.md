particular Python installation. After the version switch, you can pass all the same command line arguments to py.exe as you do to python.exe. Run the following from the Windows command line:

```
C:\Users\Al>py -3.6 -c "import sys;print(sys.version)"
3.6.6 (v3.6.6:4cf1f54eb7, Jun 27 2018, 03:37:03) [MSC v.1900 64 bit (AMD64)]

C:\Users\Al>py -2.7
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 20:25:58) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

The py.exe program is helpful when you have multiple Python versions installed on your Windows machine and need to run a specific version.

# Running Commands from a Python Program

Python’s subprocess.run() function, found in the subprocess module, can run shell commands within your Python program and then present the command output as a string. For example, the following code runs the ls -al command:

```
>>> import subprocess, locale
①>>> procObj = subprocess.run(['ls', '-al'], stdout=subprocess.PIPE)
②>>> outputStr = procObj.stdout.decode(locale.getdefaultlocale()[1])
>>> print(outputStr)
total 8
drwxr-xr-x  2 al al 4096 Aug  6 21:37  .
drwxr-xr-x 17 al al 4096 Aug  6 21:37  ..
-rw-r--r--  1 al al     0 Aug  5 15:59 spam.py
```

We pass the ['ls', '-al'] list to subprocess.run() ①. This list contains the command name ls, followed by its arguments, as individual strings. Note that passing ['ls -al'] wouldn’t work. We store the command’s output as a string in outputStr ②. Online documentation for subprocess.run() and locale.getdefaultlocale() will give you a better idea of how these functions work, but they make the code work on any operating system running Python.

# Minimizing Typing with Tab Completion

Because advanced users enter commands into computers for hours a day, modern command lines offer features to minimize the amount of typing necessary. The tab completion feature (also called command line completion or autocomplete) lets a user type the first few characters of a folder or filename and then press the TAB key to have the shell fill in the rest of the name.

For example, when you type cd c:\u and press TAB on Windows, the current command checks which folders or files in C:\ begin with u and tab completes to c:\Users. It corrects the lowercase u to U as well. (On macOS and Linux, tab completion doesn’t correct the casing.) If multiple folders