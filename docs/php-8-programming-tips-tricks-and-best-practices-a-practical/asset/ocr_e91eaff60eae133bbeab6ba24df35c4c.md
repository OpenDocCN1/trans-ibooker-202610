# Do not use FFI for speed

Because the FFI extension allows PHP direct access to C-language libraries, there is a temptation to believe that your PHP applications will suddenly operate blindingly fast, at machine-language speeds. Unfortunately, this is not the case. The FFI extension needs to first open the given C library and then parse and pseudo-compile a FFI instance before execution. The FFI extension then acts as a bridge between the C-library code and the PHP script.

It might be of relief to some readers that relatively sluggish FFI extension performance is not limited to PHP 8. Other languages suffer the same throttling effect when using their own FFI implementations. There's an excellent performance comparison, based upon the *Ary 3 benchmark*, available here: https://wiki.php.net/rfc/ffi#php_ffi_performance.

If you have a look at the table shown on the web page just referenced, you'll see that the Python FFI implementation performed the benchmark in 0.343 seconds, whereas running the same benchmark using only native Python code executed in 0.212 seconds.

Looking at the same table, the PHP 7.4 FFI extension ran the benchmark in 0.093 seconds (30 times faster than Python!), whereas the same benchmark running with just native PHP code executed in 0.040 seconds.

The next logical question is: *Why should you use the FFI extension at all?* This is covered in the next section.

# Why use the FFI extension?

The answer to the preceding question is simple: this extension is primarily designed for rapid **PHP extension prototyping**. PHP extensions are the lifeblood of the language. Without extensions, PHP is *just another programming language*.

When senior-level developers first embark upon a programming project, they need to determine the best language for the project. One key factor is how many extensions are available and how actively these are maintained. There is generally a direct correlation between the number of actively maintained extensions and the long-term success potential of a project using that language.

So, if there's a way to speed up extension development, the long-term viability of the PHP language itself is improved. The value the FFI extension brings to the PHP language is its ability to test an extension prototype directly in a PHP script without having to go through the entire compile-link-load-test cycle.