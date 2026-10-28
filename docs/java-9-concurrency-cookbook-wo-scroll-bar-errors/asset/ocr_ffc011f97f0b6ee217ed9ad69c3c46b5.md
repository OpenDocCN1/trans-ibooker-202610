13. Implement the processLines() method. It receives the string matrix, the start attribute, the end attribute, and the word attribute the task is searching for as parameters:

```
private Integer processLines(String[][] document, int start,
    int end,String word) {
```

14. For every line the task has to process, create a LineTask object to process the complete line and store them in a list of tasks:

```
List<LineTask> tasks=new ArrayList<LineTask>();
for (int i=start; i<end; i++) {
    LineTask task=new LineTask(document[i], 0,
        document[i].length, word);
    tasks.add(task);
}
```

15. Execute all the tasks in that list using the invokeAll() method:

```
invokeAll(tasks);
```

16. Sum the value returned by all these tasks and return the result:

```
int result=0;
for (int i=0; i<tasks.size(); i++) {
    LineTask task=tasks.get(i);
    try {
        result=result+task.get();
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
return result;
```

17. Implement the groupResults() method. It adds two numbers and returns the result:

```
private Integer groupResults(Integer number1,Integer number2) {
    Integer result;
    result=number1+number2;
    return result;
}
```