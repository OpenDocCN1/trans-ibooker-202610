9. Then, wait for a random period of time:

```
try {
    long duration=(long)(Math.random()*10);
    System.out.printf("%s: Waiting %d seconds for results.\n", this.name,duration);
    TimeUnit.SECONDS.sleep(duration);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

10. To generate an int value to be returned in the Result object, calculate the sum of five random numbers:

```
int value=0;
for (int i=0; i<5; i++) {
    value+=(int)(Math.random()*100);
}
```

11. Create a Result object and initialize it with the name of this Task object and the result of the operation done earlier:

```
Result result=new Result();
result.setName(this.name);
result.setValue(value);
```

12. Write a message to the console to indicate that the task has finished:

```
System.out.println(this.name+": Ends");
```

13. Return the Result object:

```
return result;
}
```

14. Finally, implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {

    public static void main(String[] args) {
```