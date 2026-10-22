
there were other exported functions in the module, they would be available too.

```
brian@tweezer ~/g/w/build> wasm3 --repl $HOME/hello.wasm
wasm3> how_old 2021 2000
Result: 21
wasm3> how_old 2021 1980
Result: 41
wasm3> $how_old 2021 2000
Error: function lookup failed ('$how_old')
wasm3> how_old 1980 2021
Result: 4294967255
wasm3>
```

Notice that I am only able to invoke the exported functions, not the inner functions. Also notice that we will fail poorly if we invert the order of parameters as anticipated. When you are building Wasm modules with higher level languages, those will make it easier to do the right thing (although it is certainly possible to write this error-checking by hand in .wat files, life is too short for that kind of nonsense). To get out of the repl, you can simply type CTRL-C or CTRL-D.

Let’s review what we just did though. We expressed some arbitrary functionality via an instruction set that targets an abstract machine. We ran it in a browser. It should work with any of the major browser on any of the major operating systems. Well, so should JavaScript. But we have also run it in a C executable running in an interactive mode on a macOS machine.

```
brian@tweezer ~/g/w/build> file wasm3
wasm3: Mach-O 64-bit executable x86_64
```

Here is it is running in the same application compiled as a Linux binary:

```
brian@bbfcfm:~/g/w/build> wasm3 --repl $HOME/hello.wasm
wasm3> how_old 2021 2000
Result: 21
wasm3> ^C
brian@bbfcfm:~/g/w/build> file wasm3

wasm3: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV),
```

```
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=b5e98161d08d2d180d0725f973b338c2a340d015, for
GNU/Linux
3.2.0, not stripped
```

There are actually several standalone WebAssembly environments written in python, Rust, Scala, OCaml, Ruby and more. Our function should be available and work in any of them.

## Running Wasm in the Browser

For our next demonstration, I will show you how to invoke the behavior in a browser using the JavaScript API. We will not introduce the API just yet, but you will see a basic example. There are more sophisticated ways of compiling the modules and parameterizing them, but first we crawl, then we walk, then we run.

In Example 2-4, we see a reusable bit of code for instantiating a WebAssembly module instance. The JavaScript API for doing so is available in any environment supporting the WebAssembly MVP, but there are other environments that do not require JavaScript such as the wasm3 runtime we just used. This code, however, will work in any WebAssembly-friendly browser⁷ or Node.js. Notice the use of the Promise-based approach. If your JavaScript environment supports async/await, you could obviously use those too.

> NOTE
> 
> The code in Example 2-4 is not the preferred way of instantiating WebAssembly modules if your browser supports the streaming compilation function. We will use it for the time being just so you can see the individual steps, but will address the preferred approach later in the book.

### Example 2-4. Instantiating a Wasm module in JavaScript

```
function fetchAndInstantiate(url, importObject) {
    return fetch(url).then(response =>
```

```
response.arrayBuffer()
).then(bytes =>
    WebAssembly.instantiate(bytes, importObject)
).then(results =>
    results.instance
);
```

Once the function is available, it is easy enough to use from HTML. In Example 2-5 you can see how that process works.

### Example 2-5. Instantiating a Wasm module from a web page

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <link rel="stylesheet" href="bootstrap.min.css">
        <title>Hello, World! (Sort of)</title>
        <script src="utils.js"></script>
    </head>
    <body>
        <div class="container">
            <h1>Hello, World! (Sort of)</h1>
            I think you are <span id="age"></span> years old.
        </div>

        <script>
            fetchAndInstantiate('hello.wasm').then(function(instance) {
                var ho = instance.exports.how_old(2021,2000);
                var ageEl = document.getElementById('age');
                ageEl.innerText=ho;
            });
        </script>
    </body>
</html>
```

In this example, we establish a <span> with an id of "age". It is currently empty. We are going to fill it with the result of invoking our WebAssembly function. There is nothing strange about the rest of our HTML file. We include our reusable instantiation code in the <head> element. Toward the bottom of this file we see an embedded <script> element which calls the fetchAndInstantiate() function. It passes in a local reference to the hello.wasm file so we will have to serve that up over HTTP as well.

The function returns a Promise. When that resolves, we receive a copy of the instantiated Wasm module instance and are able to invoke a method exposed through the module’s exports section. Notice we are passing in regular JavaScript numeric literals, but these will be just fine to pass into the function. The number 21 is returned through the invocation process and then stored in the innerText of the empty <span> we noted earlier.

We need to serve the HTML, JavaScript and Wasm module over HTTP to run in a browser. You can do that however you like, but with **python3** (or just **python** on non-Macs probably) you can start up a server and specify which port to listen on.

```
brian@tweezer ~/g/w/s/ch02> python3 -m http.server 10003
Serving HTTP on :: port 10003 (http://[::]:10003/) ...
```

If you open up your browser and point it to http://localhost:10003/index.html, you should see something along the lines of Figure 2-6 in your browser. Feel free to change the parameters in the embedded <script> element and verify that it continues to work.

![](img/16b7990eb0dbf589db177bdbd2f4a189_45_0.png)

Figure 2-6. Invoking an exported WebAssembly module function from JavaScript in a web page

We obviously have a lot left to learn, but you have now seen the equivalent of a “Hello, World!” example and hopefully understand the basics of how

# WebAssembly can work.

- 1 And most WebAssembly tutorials!
- 2 https://en.wikipedia.org/wiki/POSIX
- 3 The details of the design decisions and their motivations for the basic WebAssembly functionality are documented here:
- https://github.com/WebAssembly/design/blob/master/MVP.md
- 4 Pronounced wabbit like that wascal, Bugs Bunny.
- 5 More information about the wasm3 runtime is available here:
- https://github.com/wasm3/wasm3
- 6 If you have never used an environment like this, you should check out:
- https://en.wikipedia.org/wiki/Read–eval–print_loop
- 7 You can see which browser environments support WebAssembly (or other features) here:
- https://caniuse.com/?search=WebAssembly

## Chapter 3. WebAssembly Modules

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

> A place for everything and everything in its place.
> —17th Century Proverb

An operating system runs a program usually contained in a compiled form¹. Each operating system has its own format that defines where to start, what data is necessary, and what the instructions are for the different advertised bits of functionality actually are. WebAssembly is no different. In this chapter we are going to look at how the behavior is packaged up and how a host will know what to do with it.

It is possible that software engineers can spend their entire careers ignoring how programs are loaded and executed throught this process. Their world starts and stops at `int main(int argc, char **argv)` or `static void main(String []args)` or even `if __name__ == "__main__":`. These are well-known entry points to programs in C, Java, and Python thus this is where programmers assume responsibility for the control flow. Prior to programs being launched and after they exit, however,
