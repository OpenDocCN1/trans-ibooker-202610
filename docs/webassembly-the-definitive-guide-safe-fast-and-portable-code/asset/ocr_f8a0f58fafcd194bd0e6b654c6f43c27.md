+   7 We define a named block to return to in our loops.
+   8 We define a named loop in our block.
+   9 We break out of our loop when the $index variable equals the $n parameter.
+   10 We write the sum of the previous two elements to current location of $ptr.
+   11 We advance the $ptr variable by 4 and the $index variable by 1.
+   12 We break to the top of our loop.

Hopefully the numeric notes attached to Example 4-7 make sense, but given its complexity, it warrants a quick discussion. This is a stack-based virtual machine so all of the instructions involve manipulating the top of the stack. In <1> we import the memory defined in the JavaScript. It represents the default allocation of one page which should be enough for now. While this is a correct implementation, it is not an overly safe implementation. Bad inputs could mess up the flow, but we will be more concerned with that after we introduce higher-level language support where it is easier to handle those details.

The exported function is defined to take a parameter $n representing the number of Fibonacci numbers to calculate.⁸ We use two local variables defined at points <3> and <4>. The first represents which number we are working on and it defaults to 0. The second will act as a pointer in memory. It will serve as the index into the Memory buffer. Remember, i32 data values represent 4 bytes so every advance of $index will involve advancing $ptr by 4. We do not have the benefit of TypedArrays on this side of the interaction so we have to handle these details ourselves. Again, higher-level languages will shield us from many of these details.