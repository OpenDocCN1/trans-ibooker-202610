program and to also export the main() method so we can call it when we want. Because we are not generating a standalone binary, we also want to tell the Emscripten compiler to generate a JavaScript file called bitmap_test.js The command for the bitmap_test rule should now look like this.

> MAKEFILE TIP
> In the following code I indicate line continuation with the carriage return character (↩) so the command fits on the page. Do not type those in, just keep the line going in your file.

# Example 6-8. Modified Makefile with all of our Emscripten options

```
bitmap_test: bitmap_test.cpp bitmap_image.hpp
    $(COMPILER) $(OPTIONS) bitmap_test.js bitmap_test.cpp
$(LINKER_OPT) ↩
    -s FORCE_FILESYSTEM=1 ↩
    -s ALLOW_MEMORY_GROWTH=1 ↩
    -s INVOKE_RUN=0 ↩
    -s EXTRA_EXPORTED_RUNTIME_METHODS="['callMain']"
```

That solves the specific problems that would prevent the example from working. However, there is one remaining issue. This test runs twenty relatively time-consuming tests. As JavaScript is a single-threaded environment, while the WebAssembly module was doing its thing, the browser is likely to start freaking out about things taking too long.

We will address this eventually, but for the time being, I am just going to remove calls to the rest of the tests and only call my favorite, test20().

The main() method now looks like Example 6-9.

# Example 6-9. Main method to call one test

```
int main()
{
    test20();
    return 0;
}
```