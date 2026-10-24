
As for the second edition: in the two weeks before I revisited this section, three colleagues came to me who had made the same mistake in buffering I/O as I had in the example for the first edition.

So let’s talk about buffered I/O performance. The `InputStream.read()` and `OutputStream.write()` methods operate on a single character. Depending on the resource they are accessing, these methods can be very slow. A `FileInputStream` that uses the `read()` method will be excruciatingly slow: each method invocation requires a trip into the kernel to fetch 1 byte of data. On most operating systems, the kernel will have buffered the I/O, so (luckily) this scenario doesn’t trigger a disk read for each invocation of the `read()` method. But that buffer is held in the kernel, not the application, and reading a single byte at a time means making an expensive system call for each method invocation.

The same is true of writing data: using the `write()` method to send a single byte to a `FileOutputStream` requires a system call to store the byte in a kernel buffer. Eventually (when the file is closed or flushed), the kernel will write out that buffer to the disk.

For file-based I/O using binary data, always use `BufferedInputStream` or `BufferedOutputStream` to wrap the underlying file stream. For file-based I/O using character (string) data, always wrap the underlying stream with `BufferedReader` or `BufferedWriter`.

Although this performance issue is most easily understood when discussing file I/O, it is a general issue that applies to almost every sort of I/O. The streams returned from a socket (via the `getInputStream()` or `getOutputStream()` methods) operate in the same manner, and performing I/O one byte at a time over a socket is quite slow. Here, too, always make sure that the streams are appropriately wrapped with a buffering filter stream.

There are more subtle issues when using the `ByteArrayInputStream` and `ByteArrayOutputStream` classes. These classes are essentially just big in-memory buffers to begin with. In many cases, wrapping them with a buffering filter stream means that data is copied twice: once to the buffer in the filter stream and once to the buffer in the `ByteArrayInputStream` (or vice versa for output streams). Absent the involvement of any other streams, buffered I/O should be avoided in that case.

When other filtering streams are involved, the question of whether to buffer becomes more complicated. Later in this chapter, you’ll see an example of object serialization that involves multiple filtering streams using the `ByteArrayOutputStream`, `ObjectOutputStream`, and `GZIPOutputStream` classes.

Without the compressing output stream, the filters for that example look like this:

```
protected void makePrices() throws IOException {
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    ObjectOutputStream oos = new ObjectOutputStream(baos);
    oos.writeObject(prices);
    oos.close();
}
```

In this case, wrapping the baos stream in a BufferedOutputStream would suffer a performance penalty from copying the data one extra time.

Once we add compression, though, the best way to write the code is like this:

```
protected void makeZippedPrices() throws IOException {
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    GZIPOutputStream zip = new GZIPOutputStream(baos);
    BufferedOutputStream bos = new BufferedOutputStream(zip);
    ObjectOutputStream oos = new ObjectOutputStream(bos);
    oos.writeObject(prices);
    oos.close();
    zip.close();
}
```

Now it is necessary to buffer the output stream, because GZIPOutputStream operates more efficiently on a block of data than it does on single bytes of data. In either case, ObjectOutputStream will send single bytes of data to the next stream. If that next stream is the ultimate destination—the ByteArrayOutputStream—no buffering is necessary. If another filtering stream is in the middle (such as GZIPOutputStream in this example), buffering is often necessary.

No general rule exists about when to use a buffered stream interposed between two other streams. Ultimately, it will depend on the type of streams involved, but the likely cases will all operate better if they are fed a block of bytes (from the buffered stream) rather than a series of single bytes (from ObjectOutputStream).

The same situation applies to input streams. In this specific case, GZIPInputStream will operate more efficiently on a block of bytes; in the general case, streams that are interposed between ObjectInputStream and the original byte source will also be better off with a block of bytes.

Note that this case applies in particular to stream encoders and decoders. When you convert between bytes and characters, operating on as large a piece of data as possible will provide the best performance. If single bytes or characters are fed to encoders and decoders, they will suffer from bad performance.

For the record, not buffering the gzip streams is exactly the mistake I made when writing that compression example. It was a costly mistake, as the data in Table 12-6 shows.

**Table 12-6. Time to serialize and deserialize Stock object with compression**

| Mode | Time |
|---|---|
| Unbuffered compression/decompression | 21.3 ± 8 ms |
| Buffered compression/decompression | 5.7 ± 0.08 ms |

The failure to properly buffer the I/O resulted in as much as a four times performance penalty.

![](img/1711f142ae3cfd740ef106afbbba2046_392_0.png)

## Quick Summary

- Issues around buffered I/O are common because of the default implementation of the simple input and output stream classes.
- I/O must be properly buffered for files and sockets, as well as for internal operations like compression and string encoding.

## Classloading

The performance of classloading is the bane of anyone attempting to optimize either program startup or deployment of new code in a dynamic system.

There are many reasons for that. To begin, the class data (i.e., the Java bytecode) is typically not quickly accessible. That data must be loaded from disk or from the network, it must be found in one of several JAR files on the classpath, and it must be found in one of several classloaders. There are some ways to help this along: some frameworks cache classes they read from the network into a hidden directory so that next time it starts the same application, it can read the classes more quickly. Packaging an application into fewer JAR files will also speed up its classloading performance.

In this section, we’ll look at a new feature of Java 11 to speed up classloading.

### Class Data Sharing

*Class data sharing* (CDS) is a mechanism whereby the metadata for classes can be shared between JVMs. This can be useful for saving memory when running multiple JVMs: normally, each JVM would have its own class metadata, and the separate copies would occupy some physical memory. If that metadata is shared, only one copy needs to reside in memory.

It turns out that CDS is very useful for single JVMs because it can also improve their startup time.

Class data sharing is available in Java 8 (and previous releases), but with the restriction that it applies only to the classes in `rt.jar` and only when the serial collector is used with the client JVM. In other words, it helps somewhat on 32-bit, single-CPU, Windows desktop machines.

In Java 11, CDS is generally available on all platforms, though it doesn’t work out of the box because there is no default shared archive of the class metadata. Java 12 does have a default shared archive of the common JDK classes, so all applications will by default get some startup (and memory) benefits. In either case, we can do better by generating a more complete shared archive for our application, because in Java 11, CDS can work with any set of classes, no matter which classloader loads them and which JAR or module they are loaded from. One restriction applies: CDS works only for classes loaded from modules or JAR files. You cannot share (or quickly load) classes from a filesystem or network URL.

In a sense, this means there are two flavors of CDS: *regular CDS* (which shares the default JDK classes) and *application class data sharing*, which shares any set of classes. Application class data sharing was actually introduced in Java 10, and it worked differently than regular CDS: there were different command-line arguments for programs to use it. That distinction is now obsolete, and CDS in Java 11 and beyond works the same way regardless of the classes being shared.

The first thing required to use CDS is a shared archive of classes. As I mentioned, Java 12 comes with a default shared archive of classes, which is located in `$JAVA_HOME/lib/server/classes.jsa` (or `%JAVA_HOME%\bin\server\classes.jsa` on Windows). That archive has data for 12,000 JDK classes, so its coverage of the core classes is pretty broad. To generate your own archive, you will first need a list of all the classes for which you want to enable sharing (and hence fast loading). That list can include JDK classes and application-level classes.

There are many ways to get such a list, but the easiest is to run your application with the `-XX:+DumpLoadedClassList=filename` flag, which will produce (in *filename*) a list of all the classes that your application has loaded.

The second step is to use that class list to generate the shared archive like this:

```
```bash
$ java -Xshare:dump -XX:SharedClassListFile=filename \
    -XX:SharedArchiveFile=myclasses.jsa \
    ... classpath arguments ...
```
```

This will create a new shared archive file with the given name (here, *myclasses.jsa*) based on the list of files. You must also set up the classpath the same as you would to run the application (i.e., using either the `-cp` or `-jar` argument you would normally use to run the application).
