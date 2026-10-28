
## Creating a fork/join pool

In this recipe, you will learn how to use the basic elements of the fork/join framework. This includes the following:

- Creating a `ForkJoinPool` object to execute the tasks
- Creating a subclass of `ForkJoinTask` to be executed in the pool

The main characteristics of the fork/join framework you're going to use in this example are as follows:

- You will create `ForkJoinPool` using the default constructor.
- Inside the task, you will use the structure recommended by the Java API documentation:

```
if (problem size > default size){
    tasks=divide(task);
    execute(tasks);
} else {
    resolve problem using another algorithm;
}
```

- You will execute the tasks in a synchronized way. When a task executes two or more subtasks, it waits for their finalizations. In this way, the thread that was executing that task (called worker thread) will look for other tasks to execute, taking full advantage of their execution time.
- The tasks you're going to implement won't return any result, so you'll take the `RecursiveAction` class as the base class for their implementation.

## Getting ready

The example in this recipe has been implemented using the Eclipse IDE. If you use Eclipse or other IDEs, such as NetBeans, open it and create a new Java project.

## How to do it...

In this recipe, you are going to implement a task to update the price of a list of products. The initial task will be responsible for updating all the elements in a list. You will use a size 10 as the reference size, so if a task has to update more than 10 elements, it divides the part of the list assigned to it in two parts and creates two tasks to update the prices of the products in the respective parts.

Follow these steps to implement the example:

- 1. Create a class named Product that will store the name and price of a product:

```
public class Product {
```

- 2. Declare a private String attribute named name and a private double attribute named price:

```
private String name;
private double price;
```

- 3. Implement getter and setter methods for those fields. They are very simple to implement, so its source code is not included.

- 4. Create a class named ProductListGenerator to generate a list of random products:

```
public class ProductListGenerator {
```

- 4. Implement the generate() method. It receives an int parameter with the size of the list and returns a List<Product> object with the list of generated products:

```
public List<Product> generate (int size) {
```

- 7. Create a class named Task. Specify that it extends the RecursiveAction class:

```
public class Task extends RecursiveAction {
```

- 8. Declare a private List<Product> attribute named products:

```
private List<Product> products;
```

- 9. Declare two private int attributes named first and last. These attributes will determine the block of products this task has to process:

```
private int first;
private int last;
```

- 10. Declare a private double attribute named increment to store the increment of the price of the products:

```
private double increment;
```

- 11. Implement the constructor of the class that will initialize all the attributes of the class:

```
public Task (List<Product> products, int first, int last,
    double increment) {
    this.products=products;
    this.first=first;
    this.last=last;
    this.increment=increment;
}
```

- 12. Implement the compute() method, which will implement the logic of the task:

```
@Override
protected void compute() {
```

- 13. If the difference of the last and first attributes is less than 10 (the task has to update the price of less than 10 products), increment the price of that set of products using the updatePrices() method:

```
if (last - first<10) {
    updatePrices();
```

- 14. If the difference between the last and first attributes is greater than or equal to 10, create two new Task objects, one to process the first half of the products and the other to process the second half, and execute them in ForkJoinPool using the invokeAll() method:

```
} else {
    int middle=(last+first)/2;
    System.out.printf("Task: Pending tasks:%s\n",
                    getQueuedTaskCount());
    Task t1=new Task(products, first,middle+1, increment);
    Task t2=new Task(products, middle+1,last, increment);
    invokeAll(t1, t2);
}
```

- 15. Implement the updatePrices() method. This method updates the products that occupy the positions between the values of the first and last attributes in the list of products:

```
private void updatePrices() {
    for (int i=first; i<last; i++){
        Product product=products.get(i);
        product.setPrice(product.getPrice()*(1+increment));
    }
}
```

- 16. Implement the main class of the example by creating a class named Main and add the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

- 17. Create a list of 10000 products using the ProductListGenerator class:

```
ProductListGenerator generator=new ProductListGenerator();
List<Product> products=generator.generate(10000);
```

- 18. Create a new Task object to update the prices of all the products in the list. The parameter first takes the value 0 and the last parameter takes the value 10000 (the size of the product list):

```
Task task=new Task(products, 0, products.size(), 0.20);
```

- 19. Create a ForkJoinPool object using the constructor without parameters:

```
ForkJoinPool pool=new ForkJoinPool();
```

- 20. Execute the task in the pool using the execute() method:

```
pool.execute(task);
```

- 21. Implement a block of code that shows information about the evolution of the pool every five milliseconds, writing to the console the value of some parameters of the pool until the task finishes its execution:

```
do {
    System.out.printf("Main: Thread Count:%d\n", pool.getActiveThreadCount());
    System.out.printf("Main: Thread Steal:%d\n", pool.getStealCount());
    System.out.printf("Main: Parallelism:%d\n", pool.getParallelism());
    try {
        TimeUnit.MILLISECONDS.sleep(5);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
} while (!task.isDone());
```

- 22. Shut down the pool using the shutdown() method:

```
pool.shutdown();
```

- 23. Check if the task has finished without errors with the isCompletedNormally() method and in that case, write a message to the console:

```
if (task.isCompletedNormally()){
    System.out.printf("Main: The process has completed normally.\n");
}
```

- 24. The expected price of all the products, after the increment, is 12. Write the name and price of all the products that have a price difference of 12 to check that all of them have increased their price correctly:

```
for (int i=0; i<products.size(); i++){
    Product product=products.get(i);
    if (product.getPrice()!=12) {
        System.out.printf("Product %s: %f\n", product.getName(),product.getPrice());
    }
}
```

- 25. Write a message to indicate the finalization of the program:

```
System.out.println("Main: End of the program.\n");
```

## How it works...

In this example, you created a ForkJoinPool object and a subclass of the ForkJoinTask class that you executed in the pool. To create the ForkJoinPool object, you used the constructor without arguments, so it will be executed with its default configuration. It creates a pool with a number of threads equal to the number of processors of the computer. When the ForkJoinPool object is created, those threads are created and they wait in the pool until some tasks arrive for their execution.

Since the Task class doesn't return a result, it extends the RecursiveAction class. In the recipe, you used the recommended structure for the implementation of the task. If the task has to update more than 10 products, it divides that set of elements into two blocks, creates two tasks, and assigns a block to each task. You used the first and last attributes in the Task class to know the range of positions that this task has to update in the list of products. You used the first and last attributes to use only one copy of the product list and not create different lists for each task.

To execute the subtasks that a task creates, it calls the invokeAll() method. This is a synchronous call, and the task waits for the finalization of the subtasks before continuing (potentially finishing) its execution. While the task is waiting for its subtasks, the worker thread that was executing it takes another task waiting for execution and executes it. With this behavior, the fork/join framework offers more efficient task management than the Runnable and Callable objects themselves.

The invokeAll() method of the ForkJoinTask class is one of the main differences between the Executor and the fork/join framework. In the Executor framework, all the tasks have to be sent to the executor while in this case, the tasks include methods to execute and control the tasks inside the pool. You used the invokeAll() method in the Task class, which extends the RecursiveAction class, which in turn extends the ForkJoinTask class.

You sent a unique task to the pool to update all the lists of products using the execute() method. In this case, it's an asynchronous call, and the main thread continues its execution.

You used some methods of the ForkJoinPool class to check the status and the evolution of the tasks that are running. The class includes more methods that can be useful for this purpose. See the Monitoring a fork/join pool recipe in Chapter 9, Testing Concurrent Applications for a complete list of those methods.
