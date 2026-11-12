create an instance directly, and can’t invoke the close on it either. Before we discuss it further, let's implement the design devised so far.

resources/fpij/FileWriterEAM.java

```
```java
public class FileWriterEAM {
private final FileWriter writer;

private FileWriterEAM(final String fileName) throws IOException {
writer = new FileWriter(fileName);
}

private void close() throws IOException {
System.out.println("close called automatically...");
writer.close();
}

public void writeStuff(final String message) throws IOException {
writer.write(message);
}
//...
}
```
```

The private constructor and the private close methods are in place, along with the public method writeStuff.

# Using Higher-Order Functions

Since the programmers can't directly create an instance of FileWriterEAM, we need a factory method for them to use. Unlike the regular factory methods that create an instance and throw it across the fence, our method will yield it to users and wait for them to finish their work with it. We'll use the help of lambda expressions to do this, as we'll see soon. Let's write this method first.

resources/fpij/FileWriterEAM.java

```
```java
public static void use(final String fileName,
```
```