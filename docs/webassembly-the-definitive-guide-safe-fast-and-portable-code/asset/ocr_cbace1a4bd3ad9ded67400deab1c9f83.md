```
Type[1]:
  - type[0] (i32, i32) -> i32
Function[1]:
  - func[0] sig=0 <add>
Table[1]:
  - table[0] type=funcref initial=1
Memory[1]:
  - memory[0] pages: initial=0
Global[3]:
  - global[0] i32 mutable=0 - init i32=8
  - global[1] i32 mutable=1 - init i32=16392
  - global[2] i32 mutable=0 - init i32=16392
Export[2]:
  - func[0] <add> -> "add"
  - memory[0] -> "memory"
Elem[1]:
  - segment[0] flags=0 table=0 count=0 - init i32=1
Code[1]:
  - func[0] size=7 <add>
```

Unless you tell it otherwise, the AssemblyScript compiler will generate a Memory instance and export it for you automatically. Notice that the compiler has also exported our function just by adorning the function with the keyword export. This makes it very easy to use these compiled functions from JavaScript.

If we use our simple utils.js file from before and a simple HTML scaffolding like Example 14-5 we should see what we expect to see in the JavaScript console. There are good tools for generating more elaborate scaffolding for AssemblyScript projects but I want to keep things simple for the time being.

# Example 14-5. Simple HTML to load our AssemblyScript-base Wasm module

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="bootstrap.min.css">
    <title>Basic AssemblyScript</title>
    <script src="utils.js"></script>
  </head>
  <body>
```