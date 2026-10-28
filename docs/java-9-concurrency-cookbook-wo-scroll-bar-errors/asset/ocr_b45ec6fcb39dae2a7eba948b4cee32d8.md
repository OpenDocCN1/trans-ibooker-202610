10. Implement the auxiliary method fileProcess() that receives a File object with the full path of a file. This method checks if the file extension is equal to the one stored in the end attribute. If they are equal, add the full path of the file to the list of results:

```
private void fileProcess(File file) {
    if (file.getName().endsWith(end)) {
        results.add(file.getAbsolutePath());
    }
}
```

11. Implement a class named Task that extends the FutureTask class. You'll use List<String> as the parameterized type, as this will be the type of the data this task will return:

```
public class Task extends FutureTask<List<String>> {
```

12. Declare a private FileSearch attribute named fileSearch:

```
private FileSearch fileSearch;
```

13. Implement the constructor of this class. This constructor has two parameters: a Runnable object named runnable and a List<String> object named result. In the constructor, you have to call the constructor of the parent class, passing to it the same parameters. Then, store the runnable parameter, casting it to a FileSearch object:

```
public Task(Runnable runnable, List<String> result) {
    super(runnable, result);
    this.fileSearch=(FileSearch)runnable;
}
```

14. Override the set() method of the FutureTask class:

```
@Override
protected void set(List<String> v) {
```

15. If the parameter that it receives is null, store in it the result of calling the getResults() method of the FileSearch class:

```
v=fileSearch.getResults();
```

16. Then, call the parent's method passing the received parameter as a parameter:

```
super.set(v);
```