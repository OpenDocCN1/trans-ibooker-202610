5. Create a class named Task and specify that it implements the Runnable interface. Implement the run() method. Call the getConnection() method of the DBConnectionOK() method:

```
public class Task implements Runnable {

    @Override
    public void run() {

        System.out.printf("%s: Getting the connection...\n",
                Thread.currentThread().getName());
        DBConnectionOK connection=DBConnectionOK.getConnection();
        System.out.printf("%s: End\n",
                Thread.currentThread().getName());
    }

}
```

6. Implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

7. Create 20 Task objects and 20 threads to execute them:

```
for (int i=0; i<20; i++){
    Task task=new Task();
    Thread thread=new Thread(task);
    thread.start();
}
```

# How it works...

The key of the example is the getConnection() method and the private static class LazyDBConnection instance. When the first thread calls the getConnection() method, the LazyDBConnection class initializes the INSTANCE object by calling the constructor of the DBConnection class. This object is returned by the getConnection() method. When the rest of the threads call the getConnection() method, the object is already created, so all the threads use the same object that is created only once.

When you run the example, you will see the start and end messages of 20 tasks, but only one creation message.