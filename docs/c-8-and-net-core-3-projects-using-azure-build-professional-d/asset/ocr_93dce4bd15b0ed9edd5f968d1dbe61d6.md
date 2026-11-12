# WebAssembly

WebAssembly is something that, at the time of writing, everyone is talking about. It was first introduced in 2015. Essentially, the principle is that your browser (WebAssembly is supported by all the major browsers) can run a type of compiled code (previously, you were limited to just JavaScript).

The following link shows browser support for WebAssembly: https://caniuse.com/ #feat=wasm.

Firefox, Chrome, Edge, and Safari all support it. It's worth noting that some of the older browsers (for example, IE) don't support it, so if you're writing something that needs to be compatible with these older browsers, you may have to resort to more traditional JavaScript.

The question you may initially ask yourself is: why should I care? Hopefully, this appendix will cover the reasons why WebAssembly is braced to take over the web in the next few years.

We'll then run through two examples, one using the native WebAssembly compilation, and a second using Blazor, a technology from Microsoft that allows you to write C# and have it run in the browser.

The following topics will be covered in this chapter:

+   - Why WebAssembly?
+   - Writing WebAssembly
+   - Understanding Blazor

# # Why WebAssembly

As it currently stands, you can write JavaScript, and it will run quite happily in any browser. Given that you're reading a book on C#, you'll be (at least vaguely) familiar with the JavaScript syntax. In this section, I'll try to outline the reasons why you may choose to use WebAssembly over the possible alternatives.