8. Declare a private array of int numbers named numbers:

```
private int numbers[];
```

9. Declare two private int attributes named start and end. These attributes will determine the elements of the array this task has to process:

```
private int start, end;
```

10. Declare a private int attribute named number to store the number you are going to look for:

```
private int number;
```

11. Declare a private TaskManager attribute named manager. You will use this object to cancel all the tasks:

```
private TaskManager manager;
```

12. Declare a private int constant and initialize it to -1. It will be the returned value by the task when it doesn't find the number:

```
private final static int NOT_FOUND=-1;
```

13. Implement the constructor of the class to initialize its attributes:

```
public SearchNumberTask(int numbers[], int start, int end, int number, TaskManager manager){
    this.numbers=numbers;
    this.start=start;
    this.end=end;
    this.number=number;
    this.manager=manager;
}
```

14. Implement the compute() method. Start the method by writing a message to the console indicating the values of the start and end attributes:

```
@Override
protected Integer compute() {
    System.out.println("Task: "+start+":"+end);
```