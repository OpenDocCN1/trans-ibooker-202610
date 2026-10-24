```
for (int i = 0; i < nStrings; i++) {
    sb.append(strings[i]);
}
    bh.consume(sb.toString());
}
```

Now the results favor handcoding, which makes sense. The Java 8 implementation, in particular, has to create a new StringBuilder operation on each iteration of the loop, and even in Java 11, the overhead of creating a string on each loop (rather than building up in the string builder) takes its toll. These results are in Table 12-5.

# Table 12-5. Performance of multiple string concatenations

| Mode | 10 strings | 1,000 strings |
|---|---|---|
| JDK 11 code | 613 ± 8 ns | 2,463 ± 55 µs |
| JDK 8 code | 584 ± 8 ns | 2,602 ± 209 µs |
| String builder | 412 ± 2 ns | 38 ± 211 µs |

Bottom line: Don’t be afraid to use concatenation when it can be done on a single (logical) line, but never use string concatenation inside a loop unless the concatenated string is not used on the next loop iteration. Otherwise, always explicitly use a StringBuilder object for better performance. In Chapter 1, I argued that there are times to “prematurely” optimize, when that phrase is used in a context meaning simply “write good code.” This is a prime example.

![](bbox=[0.16, 0.53, 0.23, 0.62])

# Quick Summary

+   - One-line concatenation of strings yields good performance.
+   - For multiple concatenation operations, make sure to use StringBuilder.
+   - One-line concatenation of strings involving certain types will be significantly faster when recompiled in JDK 11.

# Buffered I/O

When I joined the Java Performance Group in 2000, my boss had just published the first ever book on Java performance, and one of the hottest topics in those days was buffered I/O. Fourteen years later, I was prepared to assume the topic was old hat and leave it out of the first edition of this book. Then, in the week I started the outline for the first edition, I filed bugs against two unrelated projects in which unbuffered I/O was greatly hampering performance. A few months later, as I was working on an example for the first edition, I scratched my head as I wondered why my “optimization” was so slow. Then I realized: stupid, you forgot to buffer the I/O correctly.