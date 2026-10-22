The proposal has a clean design and good support, but it involves a careful choreography across the HTML spec, the ES6 module spec, implementations, JavaScript bundlers, and the larger Node.js community. I suspect that it will not be much longer before we see some forward motion on this proposal.

Now that we have looked at the structural elements of a WebAssembly binary, you should feel comfortable inspecting your own and third party modules either manually or programmatically. The next step is to look at the more dynamic elements of WebAssembly modules. We will start by focusing on Memory instances as the means of emulating the power of contiguous blocks of memory in a more conventional programming runtime.

+   1 I am ignoring scripting languages for the purpose of this discussion, but the engine that runs the scripts is still going to be some kind of compiled executable.
+   2 Linux file format for executable programs:
+   https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
+   3 Windows file format for executable programs:
+   https://en.wikipedia.org/wiki/Portable_Executable
+   4 macOS file format for executable programs: https://en.wikipedia.org/wiki/Mach-O
+   5 GitHub Repo for exploring binary WebAssembly modules:
+   https://github.com/wasdk/wasmcodeexplorer
+   6 Online WebAssembly Code Explorer: https://wasdk.github.io/wasmcodeexplorer/
+   7 Many of the common formats (including WebAssembly) can be seen here:
+   https://en.wikipedia.org/wiki/List_of_file_signatures
+   8 Keep in mind we are still using the method to instantiate modules that is not recommended. One thing at a time!
+   9 Until then, if you want to explore further, the API is documented here:
+   https://webassembly.github.io/spec/js-api/
+   10 If you are interested in the low-level proposal details you can find them here:
+   https://webassembly.github.io/esm-integration/js-api/index.html#esm-integration