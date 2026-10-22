WebAssembly. As you will quickly see, the full range of browser functionality will be unlocked and usable from Rust. Even more amazingly, it will do so looking as if it were all written in Rust from that perspective. Additionally, you will be able to share Rust code with JavaScript and have it appear as JavaScript. I have worked with several inter-language bridge technologies in my day and this is one of the better ones I have seen.

The first step is to create a Rust library project using the cargo build tool. This will establish the scaffolding for the basic project.

```
brian@tweezer ~/src> cargo new --lib hello-wasm-bindgen
Created library `hello-wasm-bindgen` package
```

You can override the default code in the src/lib.rs file to be what is shown below in Example 10-9.

# Example 10-9. Our library for use with wasm-bindgen

```
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
extern {
    pub fn alert(s: &str);
}

#[wasm_bindgen]
pub fn say_hello(name: &str, whom: &str) {
    alert(&format!("Hello, {} from {}!", name, whom));
}
```

This is slightly trickier Rust than we have seen so far, but not too bad. The first line imports the contents of the wasm_bindgen::prelude module so that we can use it in our Rust code. This includes some of the binding code that will connect us to the JavaScript runtime environment.

The next line is a Rust attribute name #[wasm_bindgen]. This indicates that we plan on invoking an external function named alert(). This is among the functionality we imported with the aforementioned use statement from the prelude. If you think that this method sounds familiar, you would be right! This is ultimately going to invoke the method of the same name that you have probably called from JavaScript many times.