5. Implement the setRawResult() method. This is another abstract method of the ForkJoinTask class. As the MyWorkerTask tasks won't return any results, leave the body of this method empty:

```
@Override
protected void setRawResult(Void value) {

}
```

6. Implement the exec() method. This is the main method of the task. In this case, delegate the logic of the task to the compute() method. Calculate the execution time of this method and write it in the console:

```
@Override
protected boolean exec() {
    Date startDate=new Date();
    compute();
    Date finishDate=new Date();
    long diff=finishDate.getTime()-startDate.getTime();
    System.out.printf("MyWorkerTask: %s : %d Milliseconds to
        complete.\n",name,diff);
    return true;
}
```

7. Implement the getName() method to return the name of the task:

```
public String getName() {
    return name;
}
```

8. Declare the abstract method compute(). As mentioned earlier, this method will implement the logic of the tasks, and it must be implemented by the child classes of the MyWorkerTask class:

```
protected abstract void compute();
```

9. Create a class named Task that extends the MyWorkerTask class:

```
public class Task extends MyWorkerTask {
```

10. Declare a private array of int values named array:

```
private int array[];
```