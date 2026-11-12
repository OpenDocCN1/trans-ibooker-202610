# Reason five – existing code

Since so many languages are supported already by various tools to compile down to WASM, you may already have some code that could simply by compiled into WebAssembly. There are several anecdotal examples of this happening to great success.

Imagine that you have a game that is written entirely in C; you may be able to port this to run on the browser. I, personally, am anxiously awaiting the creation of a tool that will translate the Spectrum ZX80 Assembly language into WASM, then I can play all of the old Spectrum games on the web!

# Reason six – deployment

One of the main reasons that web development is more popular than desktop development is that the web browser solves a problem that has been the bane of our lives as programmers for years: deployment. Write a desktop application, and deploy it—you'll be surprised at how difficult that becomes to deploy when you don't have total control over the target machine.

In my opinion, this reason may be the most compelling of all of the reasons: WebAssembly provides a hybrid solution between a desktop and a web development environment.

# Reason seven – security

To be clear, WebAssembly runs in the same context as JavaScript, so it is no more or less secure than writing a web application in JavaScript. However, when you consider what you're doing, it is actually quite secure. It is far more secure than sending people an executable and asking them to run it on their machine.

I've listed some reasons why you may wish to use WebAssembly. It clearly has some advantages over the traditional web model for certain applications. However, as always with technology, it's about trade-offs.

In the next section, we'll look at how you might actually write some WebAssembly.