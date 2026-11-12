In the close method, in turn, we call the FileWriter instance's close method. If we used any other external resources in the FileWriterExample, we can clean them up here, as well. Let's make explicit use of this method in the main method.

![](bbox=[0.14, 0.2, 0.88, 0.23])

```
final FileWriterExample writerExample =
    new FileWriterExample("peekaboo.txt");
writerExample.writeStuff("peek-a-boo");
writerExample.close();
```

If we run the code now and look into the peekaboo.txt file, we'll see the peek-a-boo message. The code works, but it's far from perfect.

The explicit call to close cleans up any external resources the instance uses as soon as we indicate the instance is no longer needed. But we may not reach the call to the close method if there was an exception in the code leading up to it. We'll have to do a bit more work to ensure the call to close happens. Let's take care of that next.

# Ensuring Cleanup

We need to ensure the call to close happens whether or not there's an exception. To achieve this, we can wrap the call in a finally block.

![](bbox=[0.14, 0.74, 0.88, 0.77])

```
final FileWriterExample writerExample =
    new FileWriterExample("peekaboo.txt");
try { //Rather verbose
    writerExample.writeStuff("peek-a-boo");
} finally {
```