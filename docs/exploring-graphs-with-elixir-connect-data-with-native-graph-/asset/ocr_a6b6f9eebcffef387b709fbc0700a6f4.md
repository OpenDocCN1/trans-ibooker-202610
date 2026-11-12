the synthesized method as an argument to the referenced method, like so: `System.out.println(parameter);`. Sweet.

The code with the method reference is concise, but we have to dig into it a bit more to understand what’s going on. Once we get used to method references, our brains will know to autoparse these.

In this example, although the code is concise, the output isn’t satisfactory. We want to see letters and not numbers in their place. To fix that, let’s write a convenience method that prints an int as a letter.

![](bbox=[0.142, 0.385, 0.881, 0.41])

```
private static void printChar(int aChar) {
    System.out.println((char)(aChar));
}
```

We can use a reference to this convenience method to fix the output.

![](bbox=[0.142, 0.562, 0.881, 0.588])

```
str.chars()
    .forEach(IterateString::printChar);
```

We can continue to use the result of chars as an int, and when it’s time to print, we can convert the result to a character. The output of this version will display letters.

```
w
o
o
t
```

If we want to process characters and not int from the start, we can convert the ints to characters right after the call to the chars method, like so: