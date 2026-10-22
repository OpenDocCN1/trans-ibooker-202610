
browser into WASI-based environments. We have seen remarkably large and sophisticated software projects compiled to WebAssembly, sometimes using shims and shortcuts to deal with the platform limitations. We have also seen the active pace of the new proposals that seek to extend WebAssembly into just about every nook and cranny of our complex, heterogeneous, and ever-changing world of modern software.

And now, we have concluded with a quick survey of how support for WebAssembly is being added nearly across the board to our favorite programming languages in one form or another. It is also being adopted by and driving the features of exciting new languages. It seems like it is becoming an expectation that all of these languages will want to emit WebAssembly output to lay claim to their portion of the future.

Some of the inertia so far has been awareness, but there is now a growing consensus that WebAssembly will have far-reaching impacts at making our software safe, fast, and portable. Where there are limitations and omissions, these are generally being overcome and closed quickly both by extending the platform and developing toolchains that protect us from annoying minutiae.

I started talking about WebAssembly professionally on the NoFluffJustStuff¹⁸ tour in early 2017, just after the MVP had been finalized and browser support was becoming ubiquitous. That was clearly well before most people were ready to take advantage of this emerging platform, but I wanted to start painting a picture of what was coming so software developers can be prepared.

I have never become less excited about what this platform has in store for us; my interest has only grown. As you evaluate the various tools, technologies, and use cases we have alighted upon in this book, I hope you have caught at least some of that excitement. Things will continue to change on a weekly basis, but I hope that the majority of what I have written about is stable and worthy of your time.

I thank you for your attention and I cannot wait to see what you do with what you have learned.

- 1 The TinyGo webpage is here: https://tinygo.org
- 2 The TinyGo FAQ is here: https://tinygo.org/docs/concepts/faq
- 3 The TinyGo repo is available here: https://github.com/tinygo-org/tinygo
- 4 You can find more about this remarkable project here: https://www.jruby.org
- 5 The Artichoke web page is available here: https://artichokeruby.org
- 6 More details about what this means can be found here:
  https://en.wikipedia.org/wiki/Ruby_MRI
- 7 The irb is an interactive Ruby REPL environment: https://github.com/ruby/irb
- 8 The Artichoke Playground is located here: https://artichoke.run
- 9 The Tokamak project GitHub repo is here: https://github.com/TokamakUI/Tokamak
- 10 The wasmer-java GitHub repo is available here: https://github.com/wasmerio/wasmer-java
- 11 https://leaningtech.com/cheerpj
- 12 NBC is protecting the video, but you can listen to the skit here:
  https://www.youtube.com/watch?v=wPO8PqHGWfU
- 13 Jakub’s hands have been all over WASI, witx, Wasmtime, and more. More details are available here: http://www.jakubkonka.com
- 14 Although to be fair, it is because it is doing so much and has gotten much better over time.
- 15 The main Zig website is https://ziglang.org
- 16 Although if I have piqued your interest, “Sorry, not sorry.”
- 17 The website is located here: https://grain-lang.org
- 18 You can find out more about this traveling software carnival here: https://nofluffjuststuff.com

## Appendix A. Installing WebAssembly Tools

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

> Technology is nothing. What’s important is that you have a faith in people, that they’re basically good and smart, and if you give them tools, they’ll do wonderful things with them.
> 
> —Steve Jobs

It is unsurprising given all of the languages, tools and frameworks that we discuss in this book that there is a fair amount to install. This appendix will not be comprehensive, but will try to point you in the right direction to getting everything going. Some of the tools are easier to install on Linux or macOS, but most should work on Windows too.

### Installing WebAssembly Binary Toolkit (WABT)

The WebAssembly Binary Toolkit (WABT) provides a suite of tools for converting things too and from the various formats we have discussed as well as several others. It includes tools for dumping out details about the modules, validating their structures and more.

There are quite good instructions on the GitHub repo for building on all three major operating systems so there is no point in replicating that here. The repo is located here:

https://github.com/WebAssembly/wabt

One thing I did want to point out was that some of the tools are also available online. It shouldn’t be too surprising that WebAssembly tools might also run in the browser. If you want to try converting formats without installing the tools, you can try them out here:

https://webassembly.github.io/wabt/demo/

Additionally, there is a spin off project called Wabt.js that allows you to use much of the functionality from the toolkit in the browser. It’s GitHub repository is here:

https://github.com/AssemblyScript/wabt.js/

### Installing LLVM

LLVM looks like it would stand for something but it doesn’t. It’s just the name for an exceptionally cool modular compiler architecture that serves as the basis for languages such as Rust, Swift, Julia and many more.

LLVM is able to emit platform-specific codes even for different platforms if you install the correct tooling. It is also useful for experimenting with optimizations, running the intermediate form in a virtual machine and producing WebAssembly.

I highly recommend you install LLVM through one of the installers rather than building it from scratch as it takes forever and consumes an obscene amount of disk space. Depending on your OS, you may already have a version installed. The macOS toolchain is LLVM-based, but that version does not yet interoperate with WebAssembly.

The main website is here:
https://llvm.org

There are installers for most major operating systems so you should not have difficulty finding one that will work.

### Installing Emscripten

The Emscripten toolchain is a set of tools that wrap the LLVM tools. Originally it emitted asm.js, but now it supports WebAssembly directly. In addition to assisting with compiling existing C and C++ code, it has drop in replacements for other command line tools for building software such as Make and configure.

With its macros and compiler directives, it is fairly straightforward to simplify communicating between JavaScript host environments in the browser or Node.js. It has support for widely-used dependencies like the standard library and OpenGL.

The getting started guide has good instructions for multiple operating systems so your best bet will be to investigate this site:
https://emscripten.org/docs/getting_started/index.html

### Installing Wasm3

Wasm3 bills itself as “The fastest WebAssembly interpreter, and the most universal runtime.”

The GitHub repo is here:
https://github.com/wasm3/wasm3

Given the variety of platforms it runs on and languages it works with, I am not going to challenge them on that. It currently runs on:

- Linux, Windows, macOS, FreeBSD, Android, iOS
- OpenWrt, Yocto, Buildroot (network equipment)
- Raspberry Pi and other Single Board Computers
- A variety of microcontrollers
- Most modern Browsers

It is also doing a good job of tracking the various new proposals.

It is easy enough to build from source, but there are several installers documented here:

https://github.com/wasm3/wasm3/blob/main/docs/Installation.md

I also encourage you to check out the helpful cookbook here:

https://github.com/wasm3/wasm3/blob/main/docs/Cookbook.md

### Installing WasmTime

Wasmtime was originally a Mozilla project but is maintained by the Bytecode Alliance these days. They refer to it as “a standalone wasm-only optimizing runtime for WebAssembly and WASI”.

It is one of the most up-to-date runtimes for the various proposals and has an extensive set of programmatic libraries as you have seen throughout the book.

There is quite extensive documentation here:

https://docs.wasmtime.dev

You can find installation instructions here:

https://docs.wasmtime.dev/cli-install.html

### Installing Wasmer

Wasmer was one of the first non-browser and non-Node.js WebAssembly runtimes I encountered. It pre-dated WASI but quickly added support.
