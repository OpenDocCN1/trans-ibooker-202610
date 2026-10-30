comfortable with the content of this chapter, as misconceptions about how these primitives work will quickly get in the way of understanding the more advanced topics, or lead to you using them incorrectly.

# Talking About Memory

Not all memory is created equal. In most programming environments, your programs have access to a stack, a heap, registers, text segments, memory-mapped registers, memory-mapped files, and perhaps nonvolatile RAM. Which one you choose to use in a particular situation has implications for what you can store there, how long it remains accessible, and what mechanisms you use to access it. The exact details of these memory regions vary between platforms and are beyond the scope of this book, but some are so important to how you reason about Rust code that they are worth covering here.

# Memory Terminology

Before we dive into regions of memory, you first need to know about the difference between values, variables, and pointers. A value in Rust is the combination of a type and an element of that type’s domain of values. A value can be turned into a sequence of bytes using its type’s representation, but on its own you can think of a value more like what you, the programmer, meant." For example, the number 6 in the type u8 is an instance of the mathematical integer 6, and its in-memory representation is the byte 0x06. Similarly, the str "Hello world" is a value in the domain of all strings whose representation is its UTF-8 encoding. A value’s meaning is independent of the location where those bytes are stored.

A value is stored in a place, which is the Rust terminology for “a location that can hold a value.” This place can be on the stack, on the heap, or in a number of other locations. The most common place to store a value is a variable, which is a named value slot on the stack.

A pointer is a value that holds the address of a region of memory, so the pointer points to a place. A pointer can be dereferenced to access the value stored in the memory location it points to. We can store the same pointer in more than one variable and therefore have multiple variables that indirectly refer to the same location in memory and thus the same underlying value.

Consider the code in Listing 2-1 that illustrates these three elements.

```
let x = 42;
let y = 43;
let var1 = &x;
let mut var2 = &x;
var2 = &y;
```

Listing 2-1: Values, variables, and pointers