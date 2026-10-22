implement the functionality, there is a good chance it might have these vulnerabilities. When you have a trusted code base, being able to recompile it and use it directly is another reason to consider the topics of this chapter.

So, libsodium is basically a project designed to export the NaCl library via WebAssembly into JavaScript environments without a need to rewrite or compromising on performance. It is designed to be maintained as a WebAssembly project. I think once people get a better idea of what it means to use WebAssembly in this way, we will start to see more projects that can be used as native libraries or WebAssembly modules depending upon your configuration needs. That will be a great opportunity for code reuse. We will see another example of this approach in Chapter 10.

Until then, there is still more to learn about WebAssembly.

1 “The Adventures of Buckaroo Banzai Across the 8th Dimension” is one of the best cult sci-fi movies ever. For more details: https://en.wikipedia.org/wiki/The_Adventures_of_Buckaroo_Banzai_Across_the_8th_Dimension

2 The name is a linguistic mashup of JavaScript and the word “embiggen” made popular by “The Simpsons”.

3 Read more about Emscripten’s support for building C and C++ applications here: https://emscripten.org/docs/compiling/Building-Projects.html

4 Check out this great learning resource: https://levelupwasm.com

5 The code is available from here: https://tinyurl.com/bitmap-in-javascript

6 It is your homework assignment to make that work.

7 If you would like to know more about C++ name mangling, please check out this description: https://en.wikipedia.org/wiki/Name_mangling

8 Please note this has nothing to do with the Native Client (NaCl) we mentioned in Chapter 1. You can find more details about this one at: http://nacl.cr.yp.to

9 A ciphersuite is a collection of cryptographic primitives providing a set of capabilities to an encryption engine