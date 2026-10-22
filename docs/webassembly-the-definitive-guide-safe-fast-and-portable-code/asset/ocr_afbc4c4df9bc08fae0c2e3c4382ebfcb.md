```
let mut store = Store::new(&engine, wasi);

// Instantiate our first module which only uses WASI, then
register that
// instance with the linker since the next linking will use it.
let linking2 = linker.instantiate(&mut store, &linking2)?;
linker.instance(&mut store, "linking2", linking2)?;

// And with that we can perform the final link and the execute
the module.
let linking1 = linker.instantiate(&mut store, &linking1)?;
let run = linking1.get_typed_func::<(), (), _>(&mut store, "run")?;
run.call(&mut store, ())?;
Ok(())
}
```

Finally we link our first module to our second module, fetch the run function and invoke it as below.

```
brian@tweezer ~/g/w/s/c/hello-modlink> cargo run --release
    Compiling hello-modlink v0.1.0 (/Users/brian/git-personal/webassembly-the-definitive-guide/src/ch12/hello-modlink)
    Finished release [optimized] target(s) in 21.49s
    Running `target/release/hello-modlink`
Hello, world!
```

# Feature Testing

One of the ways you can navigate this topsy-turvy world of partial support for numerous proposals is to use a library from Google called wasm-feature-detect⁸. It’s not just a clever name. That is what it does.

It is quite straight-forward and is easilly extensible with plugin tests. They solicit input from developers who want to add checks for new proposal features they do not yet test for. Contributions involve a .wat file that provides a use of one of the new proposals to see if it works. The module will be compiled by wabt.js⁹

In Example 12-15 you can see the test for the Multi-Value return proposal.