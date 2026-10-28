+   2. Declare a private int array attribute named array to store the array of elements you want to increment:

```
private final int array[];
```

+   3. Declare two private int attributes named start and end to store the start and end positions of the block of elements this task has to process:

```
private final int start;
private final int end;
```

+   4. Implement the constructor of the class to initialize its attributes:

```
public Task (int array[], int start, int end) {
    this.array=array;
    this.start=start;
    this.end=end;
}
```

+   5. Implement the compute() method with the main logic of the task. If the task has to process more than 100 elements, first divide the elements into two parts, create two tasks to execute these parts, start its execution with the fork() method, and finally, wait for its finalization with the join() method:

```
protected void compute() {
    if (end-start>100) {
        int mid=(start+end)/2;
        Task task1=new Task(array,start,mid);
        Task task2=new Task(array,mid,end);

        task1.fork();
        task2.fork();

        task1.join();
        task2.join();
```

+   6. If the task has to process 100 elements or less, increment the elements by putting the thread to sleep for 5 milliseconds after each operation:

```
} else {
    for (int i=start; i<end; i++) {
        array[i]++;

        try {
            Thread.sleep(5);
        } catch (InterruptedException e) {
```