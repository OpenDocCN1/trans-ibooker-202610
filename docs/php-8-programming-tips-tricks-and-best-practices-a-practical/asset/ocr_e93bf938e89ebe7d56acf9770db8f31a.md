From this example, you can see that the FFI extension informational methods are able to give us an idea on how best to structure our C data in order to produce the most efficient results.

> **Tip**
> 
> For an excellent discussion on the difference between `sizeof()` and `alignof()` in the C language, see this article: https://stackoverflow.com/questions/11386946/whats-the-difference-between-sizeof-and-alignof.

You now have an understanding of what the FFI extension informational methods are and have seen some examples of their use. Let's now have a look at the FFI extension methods pertaining to infrastructure.

# Using FFI infrastructural methods

FFI extension infrastructural category methods can be thought of as *behind-the-scenes* components that support the infrastructure needed for C function binding to work properly. As we have stressed throughout this chapter, the FFI extension is needed if you wish to directly access C data structures from within a PHP application. Thus, if you need to do the equivalent of a PHP `unset()` statement to release memory, or a PHP `include()` statement to include external program code, the FFI extension infrastructural methods provide the bridge between native C data and PHP.

Table 4.4, shown here, summarizes methods in this category:

| FFI Method | Returns | Notes |
|---|---|---|
| `addr()` | `FFI\CData` | Creates pointer to C data |
| `cast()` | `FFI\CData` | Performs a C language type cast |
| `free()` | `void` | Releases an FFI pointer. There is no direct equivalent in PHP |
| `memcpy()` | `void` | Copies specified bytes from one `FFI\CData` instance to another. |
| `memset()` | `void` | Populates a `FFI\CData` instance with `$size` bytes consisting of `$value`. |

Table 4.4 – FFI class infrastructural methods