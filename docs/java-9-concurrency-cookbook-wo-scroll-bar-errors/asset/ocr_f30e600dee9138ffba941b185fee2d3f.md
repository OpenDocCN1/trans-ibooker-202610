+   6. Now let's implement the main class of the example. Implement a class called `Main` that contains the `main()` method:

```
public class Main {
    public static void main(String[] args) {
```

+   7. Create and initialize an object of the `FileSearch` class and thread to execute its task. Then start executing the thread. I have used a Windows operating system route. If you work with other operating systems, such as Linux or iOS, change the route to the one that exists on your operating system:

```
FileSearch searcher = new FileSearch("C:\\Windows",
    "explorer.exe");
Thread thread=new Thread(searcher);
thread.start();
```

+   8. Wait for 10 seconds and interrupt the thread:

```
try {
    TimeUnit.SECONDS.sleep(10);
} catch (InterruptedException e) {
    e.printStackTrace();
}
thread.interrupt();
}
```

+   9. Run the example and see the results.

# How it works...

The following screenshot shows the result of an execution of this example. You can see how the `FileSearch` object ends its execution when it detects that it has been interrupted.

![](bbox=[0.22, 0.67, 0.78, 0.78])