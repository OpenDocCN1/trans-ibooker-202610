23. Then, add the values returned by both the tasks using the groupResults() method. Finally, return the result calculated by the task:

```
try {
    result=groupResults(task1.get(),task2.get());
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
return result;
```

24. Implement the count() method. It receives the string array with the complete line, the start attribute, the end attribute, and the word attribute the task is searching for as parameters:

```
private Integer count(String[] line, int start, int end,
    String word) {
```

25. Compare the words stored in the positions between the start and end attributes with the word attribute the task is searching for, and if they are equal, increment the counter variable:

```
int counter;
counter=0;
for (int i=start; i<end; i++){
    if (line[i].equals(word)){
        counter++;
    }
}
```

26. To slow the execution of the example, put the task to sleep for 10 milliseconds:

```
try {
    Thread.sleep(10);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

27. Return the value of the counter variable:

```
return counter;
```