Now, copy the code from Example 2-3 into the lower left panel. This will invoke the WebAssembly JavaScript API available in most modern browsers (and Node.js). We will discuss it more later, but for now we are retrieving the bytes of the binary module (available here via the wasmModule variable) and getting a reference to the how_old function so we can call it. As you can see, this function can be invoked like any other JavaScript function. The result of doing so will be printed out via console.log() to the lower right panel.

# Example 2-3. Some JavaScript to invoke our function

```
const wasmInstance = new WebAssembly.Instance(wasmModule, {});
const { how_old } = wasmInstance.exports;
console.log(how_old(2021, 2000));
```

If everything goes well, you should see something like the screen shot in Figure 2-5. Try changing the dates for the current year and birth year parameters and make sure that our math is correct.

![](bbox=[0.17, 0.485, 0.83, 0.84])

Figure 2-5. Converting a WebAssembly text file into a binary file and executing it