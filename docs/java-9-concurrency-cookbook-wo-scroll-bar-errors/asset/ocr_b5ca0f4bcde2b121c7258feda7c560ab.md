4. Implement the getLine() method that returns the line determined by the index attribute and increases its value:

```
public String getLine(){
    if (this.hasMoreLines()) {
        System.out.println("Mock: " + (content.length-index));
        return content[index++];
    }
    return null;
}
```

5. Now implement a class named Buffer that will implement the buffer shared by both the producers and consumers:

```
public class Buffer {
```

6. This class has six attributes:

+   - A LinkedList<String> attribute named buffer that will store the shared data. For example:
+   
+     private final LinkedList<String> buffer;
+   - An int type named maxSize that will store the length of the buffer. For example:
+   
+     private final int maxSize;
+   - A ReentrantLock object called lock that will control access to the blocks of code that modify the buffer. For example:
+   
+     private final ReentrantLock lock;
+   - Two Condition attributes named lines and space. For example:
+   
+     private final Condition lines;
+   
+     private final Condition space;
+   - A boolean type called pendingLines that will indicate whether there are lines in the buffer. For example:
+   
+     private boolean pendingLines;