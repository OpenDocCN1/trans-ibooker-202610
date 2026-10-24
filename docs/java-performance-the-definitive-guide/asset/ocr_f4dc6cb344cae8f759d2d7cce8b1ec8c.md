# Table 7-2. Sizes in bytes of simple objects

|  | Shallow size | Deep size | Retained size |
|---|---|---|---|
| A | 16 | 16 | 16 |
| B | 24 | 216 | 24 |
| C | 24 | 200 | 200 |

In class B, defining the Locale reference adds 8 bytes to the object size, but at least in that example, the Locale object is shared among other classes. If the Locale object is never needed by the class, including that instance variable will waste only the additional bytes for the reference. Still, those bytes add up if the application creates a lot of instances of class B.

On the other hand, defining and creating a ConcurrentHashMap consumed additional bytes for the object reference, plus additional bytes for the hash-map object. If the hash map is never used, instances of class C are wasteful.

Defining only required instance variables is one way to save space in an object. The less obvious case involves using smaller data types. If a class needs to keep track of one of eight possible states, it can do so using a byte rather than an int—potentially saving 3 bytes. Using float instead of double, int instead of long, and so on, can help save memory, particularly in classes that are frequently instantiated. As discussed in Chapter 12, using appropriately sized collections (or using simple instance variables instead of collections) achieves similar savings.

# Object Alignment and Object Sizes

The classes mentioned in Table 7-2 all contain an extra integer field that was not referenced in the discussion. Why is that there?

In truth, that variable serves the purpose of making the discussion of those classes easier to follow: class B contains 8 more bytes than class A, which is what we’d expect (and which makes the point more clearly).

That glosses over an important detail: object sizes are always padded so that they are a multiple of 8 bytes. Without the definition of i in class A, instances of A still consume 16 bytes—the 4 bytes are just used for padding the object size to a multiple of 8, rather than being used to hold the reference to i. Without the definition of i, instances of class B would consume only 16 bytes—the same as A, even though B has that extra object reference. That padding is also why an instance of B is 8 bytes larger than an instance of A even though it contains only one additional (4-byte) reference.

The JVM will also pad objects that have an uneven number of bytes so that arrays of that object fit neatly along whatever address boundaries are optimal for the underlying architecture.