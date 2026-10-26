```
int idx = j + i * 256;
table[idx] = !isspace(j) && isspace(i);
}
```

The loop to count the words becomes the following (the full code can be found in Snippet15.cpp):

```
int count = 0;
for(int i = 0; i < s.size() - 1; ++i)
{
    // grab the 2 bytes at s[i] as a 16 bit value
    unsigned short idx;
    memcpy(&idx, &s[i], 2);
    count += table[idx];
}
```

We read each character of the string as a single 16-bit value. Directly casting a pointer from char* to another type and dereferencing it is undefined behavior - the correct way to do this is to use memcpy(). The compiler is smart enough to use the CPU memory access instructions rather than actually call memcpy() for 2 bytes. We have ended up with the loop containing no conditional statement, which should make it much faster. Remember that X86 architecture is little-endian - so a 16-bit value read from a character array will have the first character as its LSB and the second as the MSB.

+   7. Now, time the code we wrote:

```
$ g++ -O3 Snippet15.cpp SnippetWC.cpp Timer.cpp
$ ./a.out data.txt
```

![](bbox=[0.11, 0.66, 0.92, 0.74])

Figure 8.44: Output of the code in Snippet15.cpp

This larger lookup table resulted in a 1.8x speed improvement for wordCount(). Let's step back and look at this from another angle so that we can use existing the standard library effectively. The advantages of this are two-fold - firstly, the code is less prone to errors, and secondly, we could take advantage of the parallelization available with some compilers.