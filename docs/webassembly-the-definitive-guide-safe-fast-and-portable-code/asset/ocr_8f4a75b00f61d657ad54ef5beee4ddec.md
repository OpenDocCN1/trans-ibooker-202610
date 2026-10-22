```
using var module = Module.FromTextFile(engine, "hello.wat");
using var linker = new Linker(engine);
using var store = new Store(engine);

linker.Define(
    "hello",
    "world",
    Function.FromCallback(store,
        () => Console.WriteLine("I like soccer and shishkabobs."))
    );

var instance = linker.Instantiate(store, module);
var exec = instance.GetFunction(store, "exec");
exec.Invoke(store);
}
```

Running the dotnet command line will show us what you expect.

```
brian@tweezer ~/g/w/s/wasmtime-dotnet> dotnet run
I like soccer and shishkabobs.
```

There are plenty of additional examples at the GitHub repo linked in the footnote above including other examples of Wasi integration. There are similar examples available for using .NET with the Wasmer runtime as well. The main point is that WebAssembly can easily be invoked from and interact with a .NET environment. The next use case is a bit of a departure but still an up-and-coming combination of these technologies.

# Blazor

Microsoft has another vision of WebAssembly that aligns with their renewed interest in providing tools and frameworks for developers on all major platforms. The Blazor framework⁶ allows developers to target the web with interactive components and applications written in C#. Development can be done with a variety of tools on Windows, macOS, and Linux.