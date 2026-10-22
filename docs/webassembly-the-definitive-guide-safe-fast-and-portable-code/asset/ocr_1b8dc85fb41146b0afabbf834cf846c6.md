```
return 0;
}
```

1 puts() calls the JavaScript function with a char *.

At long last, we see how this works. Our function calls printf() which calls puts() which is defined to call the provided function. I am not going to describe how that gets hooked in right now, but I hope the result is still satisfying. There is more to know about using C/C++ with WebAssembly, but that is the subject of the forthcoming chapters. Until then, you have just crossed an important chasm for understanding how WebAssembly works behind the scenes. Next we will learn how to port existing software to run in the browser.

+   1 C’s history is an integral part of our industry and the development of modern operating systems. https://en.wikipedia.org/wiki/C_(programming_language)

+   2 One of the most popular programming books ever:
+   https://en.wikipedia.org/wiki/The_C_Programming_Language

+   3 And the lion’s share of security issues!

+   4 LLVM used to stand for Low-Level Virtual Machine, but these days it just means LLVM. Find out more at https://llvm.org

+   5 Loop unrolling is a common optimization in many programming languages.
+   https://en.wikipedia.org/wiki/Loop_unrolling

+   6 There is a useful C tutorial at https://learn-c.org if you do not mind some annoying ads. Otherwise, Steve Oualline’s Practical C Programming or the aforementioned The C Programming Language are good introductions.

+   7 nm is a common tool in the world of C to print out the contents of binary files such as this.

+   8 Sometimes referred to as “ten pounds of manure in a five pound bag”. It will not fit!

+   9 You can find Petter’s sample project here: https://github.com/PetterS/clang-wasm

+   10 It is outside of the scope of this book to cover these details but you can find more here if you are interested: https://en.wikipedia.org/wiki/Name_mangling

+   11 Merge Sort is a utility sorting algorithm with O(n log n) complexity. It is easy to implement which is why I chose it. You can find more details here:
+   https://en.wikipedia.org/wiki/Merge_sort

+   12 Petter’s example at the previously listed GitHub repo does more with strings moving back and forth from JavaScript and C