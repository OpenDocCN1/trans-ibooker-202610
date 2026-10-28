```
Event e = deque.getLast();
difference = date.getTime() - e.getDate().getTime();
if (difference > 10000) {
    System.out.printf("Cleaner: %s\n",e.getEvent());
    deque.removeLast();
    delete=true;
}
} while (difference > 10000);
if (delete){
    System.out.printf("Cleaner: Size of the queue: %d\n", deque.size());
}
```

+   9. Now implement the main class. Create a class called Main with a main() method:

```
public class Main {
    public static void main(String[] args) {
```

+   10. Create the queue to store the events using the Deque class:

```
Deque<Event> deque=new ConcurrentLinkedDeque<Event>();
```

+   11. Create and start as many WriterTask threads as available processors have the JVM and one CleanerTask method:

```
WriterTask writer=new WriterTask(deque);
for (int i=0; i< Runtime.getRuntime().availableProcessors(); i++){
    Thread thread=new Thread(writer);
    thread.start();
}
CleanerTask cleaner=new CleanerTask(deque);
cleaner.start();
```

+   12. Run the program and see the results.

# How it works...

If you analyze the output of one execution of the program, you would see how the queue begins to grow until it has a size of, in our case, 40 events. Then, its size will vary around 40 events it has grown up to until the end of the execution. This size may depend on the number of cores of your machine. I have executed the code in a four-core processor, so we launch four WriterTask tasks.