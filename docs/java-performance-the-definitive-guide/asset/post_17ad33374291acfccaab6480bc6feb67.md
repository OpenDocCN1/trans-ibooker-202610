
- Blocked Thread Visibility 302
- Summary 306

## 10. Java Servers 307

- Java NIO Overview 307
- Server Containers 309
- Tuning Server Thread Pools 309
- Async REST Servers 311
- Asynchronous Outbound Calls 314
- Asynchronous HTTP 315
- JSON Processing 322
  - An Overview of Parsing and Marshaling 323
  - JSON Objects 324
  - JSON Parsing 325
- Summary 327

## 11. Database Performance Best Practices 329

- Sample Database 330
- JDBC 330
  - JDBC Drivers 330
  - JDBC Connection Pools 333
  - Prepared Statements and Statement Pooling 334
  - Transactions 336
  - Result Set Processing 345
- JPA 347
  - Optimizing JPA Writes 347
  - Optimizing JPA Reads 349
  - JPA Caching 353
- Spring Data 360
- Summary 361

## 12. Java SE API Tips 363

- Strings 363
  - Compact Strings 363
  - Duplicate Strings and String Interning 364
  - String Concatenation 371
- Buffered I/O 374
- Classloading 377
  - Class Data Sharing 377
- Random Numbers 381
- Java Native Interface 383
- Exceptions 386
- Logging 390
- Java Collections API 392
  - Synchronized Versus Unsynchronized 392
  - Collection Sizing 394
  - Collections and Memory Efficiency 395
- Lambdas and Anonymous Classes 397
- Stream and Filter Performance 399
  - Lazy Traversal 399
- Object Serialization 402
  - Transient Fields 403
  - Overriding Default Serialization 403
  - Compressing Serialized Data 406
  - Keeping Track of Duplicate Objects 408
  - Summary 411

- A. Summary of Tuning Flags 413
- Index 423

## Preface

When O’Reilly first approached me about writing a book on Java performance tuning, I was unsure. Java performance, I thought—aren’t we done with that? Yes, I still work on improving the performance of Java (and other) applications on a daily basis, but I like to think that I spend most of my time dealing with algorithmic inefficiencies and external system bottlenecks rather than on anything directly related to Java tuning.

A moment’s reflection convinced me that I was (as usual) kidding myself. It is certainly true that end-to-end system performance takes up a lot of my time, and that I sometimes come across code that uses an $O(n^2)$ algorithm when it could use one with $O(\log N)$ performance. Still, it turns out that every day I think about garbage collection (GC) performance, or the performance of the JVM compiler, or how to get the best performance from Java APIs.

That is not to minimize the enormous progress that has been made in the performance of Java and JVMs over the past 20-plus years. When I was a Java evangelist at Sun during the late 1990s, the only real “benchmark” available was CaffeineMark 2.0 from Pendragon software. For a variety of reasons, the design of that benchmark quickly limited its value; yet in its day, we were fond of telling everyone that Java 1.1.8 performance was eight times faster than Java 1.0 performance based on that benchmark. And that was true—Java 1.1.8 had an actual just-in-time compiler, whereas Java 1.0 was pretty much completely interpreted.

Then standards committees began to develop more rigorous benchmarks, and Java performance began to be centered around them. The result was a continuous improvement in all areas of the JVM—garbage collection, compilations, and within the APIs. That process continues today, of course, but one of the interesting facts about performance work is that it gets successively harder. Achieving an eightfold increase in performance by introducing a just-in-time compiler was a straightforward matter of engineering, and even though the compiler continues to improve, we’re not going to see an improvement like that again. Parallelizing the garbage collector was a huge performance improvement, but more recent changes have been more incremental.

This is a typical process for applications (and the JVM itself is just another application): in the beginning of a project, it's easy enough to find architectural changes (or code bugs) that, when addressed, yield huge performance improvements. In a mature application, finding such performance improvements is rare.

That precept was behind my original concern that, to a large extent, the engineering world might be done with Java performance. A few things convinced me I was wrong. First is the number of questions I see daily about how this or that aspect of the JVM performs under certain circumstances. New engineers come to Java all the time, and JVM behavior remains complex enough in certain areas that a guide to its operation is still beneficial. Second is that environmental changes in computing seem to have altered the performance concerns that engineers face today.

Over the past few years, performance concerns have become bifurcated. On the one hand, very large machines capable of running JVMs with very large heaps are now commonplace. The JVM has moved to address those concerns with a new garbage collector (G1), which—as a new technology—requires a little more hand-tuning than traditional collectors. At the same time, cloud computing has renewed the importance of small, single-CPU machines: you can go to Oracle or Amazon or a host of other companies and cheaply rent a single CPU machine to run a small application server. (You're not actually getting a single-CPU machine: you're getting a virtual OS image on a very large machine, but the virtual OS is limited to using a single CPU. From the perspective of Java, that turns out to be the same as a single-CPU machine.) In those environments, correctly managing small amounts of memory turns out to be quite important.

The Java platform also continues to evolve. Each new edition of Java provides new language features and new APIs that improve the productivity of developers—if not always the performance of their applications. Best practice use of these language features can help differentiate between an application that sizzles and one that plods along. And the evolution of the platform brings up interesting performance questions: there is no question that using JSON to exchange information between two programs is much simpler than coming up with a highly optimized proprietary protocol. Saving time for developers is a big win—but making sure that productivity win comes with a performance win (or at least breaks even) is the real goal.

### Who Should (and Shouldn't) Read This Book

This book is designed for performance engineers and developers who are looking to understand how various aspects of the JVM and the Java APIs impact performance.

If it is late Sunday night, your site is going live Monday morning, and you're looking for a quick fix for performance issues, this is not the book for you.

If you are new to performance analysis and are starting that analysis in Java, this book can help you. Certainly my goal is to provide enough information and context that novice engineers can understand how to apply basic tuning and performance principles to a Java application. However, system analysis is a broad field. There are a number of excellent resources for system analysis in general (and those principles, of course, apply to Java), and in that sense, this book will ideally be a useful companion to those texts.

At a fundamental level, though, making Java go really fast requires a deep understanding of how the JVM (and Java APIs) actually work. Hundreds of Java tuning flags exist, and tuning the JVM has to be more than an approach of blindly trying them and seeing what works. Instead, my goal is to provide detailed knowledge about what the JVM and APIs are doing, with the hope that if you understand how those things work, you'll be able to look at the specific behavior of an application and understand why it is performing badly. Understanding that, it becomes a simple (or at least simpler) task to get rid of undesirable (badly performing) behavior.

One interesting aspect to Java performance work is that developers often have a very different background than engineers in a performance or QA group. I know developers who can remember thousands of obscure method signatures on little-used Java APIs but who have no idea what the flag -Xmn means. And I know testing engineers who can get every last ounce of performance from setting various flags for the garbage collector but who can barely write a suitable “Hello, World” program in Java.

Java performance covers both of these areas: tuning flags for the compiler and garbage collector and so on, and best-practice uses of the APIs. So I assume that you have a good understanding of how to write programs in Java. Even if your primary interest is not in the programming aspects of Java, I do spend a fair amount of time discussing programs, including the sample programs used to provide a lot of the data points in the examples.

Still, if your primary interest is in the performance of the JVM itself—meaning how to alter the behavior of the JVM without any coding—then large sections of this book should still be beneficial to you. Feel free to skip over the coding parts and focus on the areas that interest you. And maybe along the way, you'll pick up some insight into how Java applications can affect JVM performance and start to suggest changes to developers so they can make your performance-testing life easier.
