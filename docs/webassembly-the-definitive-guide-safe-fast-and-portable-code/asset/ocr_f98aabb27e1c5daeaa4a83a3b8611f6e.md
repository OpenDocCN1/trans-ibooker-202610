```
brian@tweezer ~/s/hello-wasm-bindgen> wasm-pack build --target web
[INFO]:  Checking for the Wasm target...
[INFO]:  Compiling to Wasm...
    Compiling hello-wasm-bindgen v0.1.0 (/Users/brian/src/hello-wasm-bindgen)
        Finished release [optimized] target(s) in 0.26s
[INFO]: Optimizing wasm binaries with `wasm-opt`...
[INFO]:     Done in 0.73s
[INFO]:     Your wasm pkg is ready to publish at
/Users/brian/src/hello-wasm-bindgen/pkg.
brian@tweezer ~/s/hello-wasm-bindgen> ls -laF pkg
total 72
drwxr-xr-x   8 brian  staff   256 May 10 17:20 ./
drwxr-xr-x   9 brian  staff   288 May 10 17:20 ../
-rw-r--r--   1 brian  staff     1 May 10 17:20 .gitignore
-rw-r--r--   1 brian  staff   861 May 10 17:20 hello_wasm_bindgen.d.ts
-rw-r--r--   1 brian  staff  4026 May 10 17:20 hello_wasm_bindgen.js
-rw-r--r--   1 brian  staff 15786 May 10 17:20 hello_wasm_bindgen_bg.wasm
-rw-r--r--   1 brian  staff   291 May 10 17:20 hello_wasm_bindgen_bg.wasm.d.ts
-rw-r--r--   1 brian  staff   266 May 10 17:20 package.json
```

We have used the --target web flag to indicate we would like our package to be loadable in a browser. Other options including using Webpack to bundle everything up or targeting Node.js or Deno as we will see shortly. In the pkg directory, you will see the generated JavaScript, our Wasm module, and a package.json file. There are also TypeScript Declaration files for our code as well. If you use wasm-objdump and checkout the Export section of our module, you will see the following:

```
Export[4]:
  - memory[0] -> "memory"
  - func[19] <say_hello> -> "say_hello"
  - func[34] <__wbindgen_malloc> -> "__wbindgen_malloc"
  - func[38] <__wbindgen_realloc> -> "__wbindgen_realloc"
```

This includes a Memory instance, our exported method and some memory allocation functions.