and a ‘really bad idea’⁵ 5 second delay in main to allow the threads to finish.

```
public class RequestCounter {
    final static private int NUMTHREADS = 50000;
    private int count = 0;

    public void inc() {
        count++;
    }

    public int getVal() {
        return this.count;
    }

    public static void main(String[] args) throws InterruptedException {
        final RequestCounter counter = new RequestCounter();

        for (int i = 0; i < NUMTHREADS; i++) {
            // lambda runnable creation
            Runnable thread = () -> {counter.inc(); };
            new Thread(thread).start();
        }

        Thread.sleep(5000);
        System.out.println("Value should be " + NUMTHREADS + "It is: " + counter.getVal());
    }
}
```

What you can do at home is clone this code from the book github repo, run this code a few times and see what results you get. In 10 executions my mean was 49995. I didn’t once get the correct answer of 50000. Weird.

Why?

The answer lies in how abstract, high-level programming language statements, in Java in this case, are executed on a machine. In this example, to perform an increment of a counter, the CPU must (1) load the current value into a register, (2) increment the register value, and (3) write the results back to the original memory location. This simple increment is actually a sequence of three machine-level operations.