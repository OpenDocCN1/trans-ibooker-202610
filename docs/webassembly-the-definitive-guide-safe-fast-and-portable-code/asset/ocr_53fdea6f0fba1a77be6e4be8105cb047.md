```
// src/greeter.ts
function greeter(person: string) {
  return "Hello, " + person;
}
let user = [0, 1, 2];
document.body.textContent = greeter(user);
```

The TypeScript compiler is easily able to stop this mistake from happening at compile time, a much better time to find errors!

```
brian@tweezer ~/s/t/hello-ts> tsc src/greeter.ts
src/greeter.ts:7:37 - error TS2345: Argument of type 'number[]'
is not assignable to parameter
of type 'string'.
7 document.body.textContent = greeter(user);
                    ~~~~
Found 1 error.
```

Even better, TypeScript-aware development environments will be able to flag the error as soon as you enter the code. VS Code from Microsoft and other popular IDEs have very nice support for developer assistance using this language.

# “Definitely not a TypeScript to WebAssembly compiler”

This section header is a direct quotation from the AssemblyScript GitHub page. It is part of an attempt to be clear about the role it plays and that it will not be possible to simply compile a typical TypeScript codebase to WebAssembly. That being said, AssemblyScript is an attempt to provide a JavaScript-like (and, more to the point TypeScript-like) experience but with the performance and security WebAssembly offers. It is largely considered a useful way of building higher-performance Web-based applications without having to rely on C, C++ or Rust. It should be a relatively straightforward transition for existing Web developers to move into and benefit from this platform.