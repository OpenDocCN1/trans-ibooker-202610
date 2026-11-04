Another favorable aspect of the PHP JIT implementation is that it doesn't produce any **Intermediate Representation (IR)** code. In contrast, the **PyPy VM** used to run Python code using JIT compiler technology, has to first produce IR code in a **graph structure**, used for flow analysis and optimization, before the actual machine code is produced. The DynASM core in the PHP JIT doesn't require this extra step, resulting in greater performance than is possible in other interpreted programming languages.

> **Tip**
> 
> For more information on DynASM, have a look at this website: `https://luajit.org/dynasm.html`. Here's an excellent overview of how the PHP 8 JIT operates: `https://www.zend.com/blog/exploring-new-php-jit-compiler`. You can also read the official JIT RFC here: `https://wiki.php.net/rfc/jit`.

Now that you have an idea of how the JIT compiler fits into the general flow of a PHP program execution cycle, it's time to learn how to enable it.

# Enabling the JIT compiler

Because the primary function of the JIT compiler is to cache machine code, it operates as an independent part of the OPcache extension. OPcache serves as a gateway to both enable JIT functionality as well as to allocate memory to the JIT compiler from its own allotment. Therefore, in order to enable the JIT compiler, you must first enable OPcache (see the previous section, *Understanding the operation of PHP OPcache*).

In order to enable the JIT compiler, you must first confirm that PHP has been compiled with the `--enable-opcache-jit` configuration option. You are then in a position to enable or disable the JIT compiler by simply assigning a non-zero value to the `php.ini` file's `opcache.jit_buffer_size` directive.

Values are specified either as an integer – in which case, the value represents the number of bytes; a value of zero (the default), which disables the JIT compiler; or you can assign a number followed by any of the following letters:

+   - K: Kilobytes
+   - M: Megabytes
+   - G: Gigabytes