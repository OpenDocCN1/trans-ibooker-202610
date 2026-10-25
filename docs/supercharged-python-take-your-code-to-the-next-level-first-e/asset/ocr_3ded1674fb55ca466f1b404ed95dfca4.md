# 8.5 USING THE “WITH” KEYWORD

The most obvious way to do file operations is to open a file,
perform file I/O, and close the file. But what if an exception is
raised in the middle of a file I/O read? The program abruptly
ends without tying up loose ends and politely closing down
resources.

A nice shortcut is to employ a **with** statement. The action is
to open a file and permit access through a variable. If, during
execution of the block, an exception is raised, the file is
automatically closed, so that no file handles remain open.

![](bbox=[0.1465, 0.4094, 0.2539, 0.4891])

Click here to view code image

```
with open(filename, mode_str) as file_obj:
    statements
```

In this syntax, the *filename* and *mode_str* arguments have
the same meaning they do in the **open** statement as described
in the next section. The *file_obj* is a variable name that you
supply; this variable gets assigned the file object returned by the
**open** statement. The statements are then executed, until (of
course) an exception is raised.
Here’s an example that reads a text file using the **with**
keyword:

Click here to view code image