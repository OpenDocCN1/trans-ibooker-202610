
5. Create a class named Task and specify that it implements the Runnable interface:

```
public class Task implements Runnable {
```

6. Declare a private ConcurrentSkipListMap attribute, parameterized by the String and Contact classes, named map:

```
private final ConcurrentSkipListMap<String, Contact> map;
```

7. Declare a private String attribute named id to store the ID of the current task:

```
private final String id;
```

8. Implement the constructor of the class to store its attributes:

```
public Task (ConcurrentSkipListMap<String, Contact> map,String id){
    this.id=id;
    this.map=map;
}
```

9. Implement the run() method. It stores 1,000 different contacts in the map using the ID of the task and an incremental number to create Contact objects. Use the put() method to store the contacts in the map:

```
@Override
public void run() {
    for (int i=0; i<1000; i++) {
        Contact contact=new Contact(id, String.valueOf(i+1000));
        map.put(id+contact.getPhone(), contact);
    }
}
```

10. Implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

11. Create a ConcurrentSkipListMap object, parameterized by the String and Contact classes, named map:

```
ConcurrentSkipListMap<String, Contact> map = new
    ConcurrentSkipListMap<>();
```

12. Create an array for 26 Thread objects to store all the Task objects that you're going to execute:

```
Thread threads[]=new Thread[26];
int counter=0;
```

13. Create and launch 26 task objects and assign a capital letter to the ID of each task:

```
for (char i='A'; i<='Z'; i++) {
    Task task=new Task(map, String.valueOf(i));
    threads[counter]=new Thread(task);
    threads[counter].start();
    counter++;
}
```

14. Wait for the finalization of the threads using the join() method:

```
for (Thread thread : threads){
    try {
        thread.join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

15. Get the first entry of the map using the firstEntry() method. Write its data to the console:

```
System.out.printf("Main: Size of the map: %d\n",map.size());

Map.Entry<String, Contact> element;
Contact contact;

element=map.firstEntry();
contact=element.getValue();
System.out.printf("Main: First Entry: %s: %s\n", contact.getName(), contact.getPhone());
```

16. Get the last entry of the map using the lastEntry() method. Write its data to the console:

```
element=map.lastEntry();
contact=element.getValue();
System.out.printf("Main: Last Entry: %s: %s\n", contact.getName(), contact.getPhone());
```

The following screenshot shows the output of an execution of the program:

![](img/cc06c59cd21f558f43e9475d7f212170_345_0.png)

## There's more...

The ConcurrentSkipListMap class has other interesting methods. Some of them are as follows:

- headMap(K toKey): Here, K is the class of the key values used in the parameterization of the ConcurrentSkipListMap object. This method returns a submap of the first elements of the map with the elements that have a key smaller than the one passed as a parameter.
- tailMap(K fromKey): Here, K is the class of the key values used in the parameterization of the ConcurrentSkipListMap object. This method returns a submap of the last elements of the map with the elements that have a key greater than the one passed as a parameter.
- putIfAbsent(K key, V Value): This method inserts the value specified as a parameter and also the key specified as a parameter if it doesn't exist in the map.
- pollLastEntry(): This method returns and removes a Map.Entry object with the last element of the map.
- replace(K key, V Value): This method replaces the value associated with the key specified as a parameter if this key exists in the map.

## See also

- The *Using non-blocking thread-safe deques* recipe in this chapter

## Using thread-safe HashMaps

A hash table is a data structure that allows you to map a key to a value. Internally, it usually uses an array to store the elements and a hash function to calculate the position of the element in the array, using its key. The main advantage of this data structure is that the insert, delete, and search operations are very fast here, so it's very useful in situations when you have to carry out a lot of search operations.

The Java API provides different hash table implementations through the `Map` and `ConcurrentMap` interfaces. The `ConcurrentMap` interface provides thread-safety and atomic guarantees to all the operations, so you can use them in concurrent applications. The `ConcurrentHashMap` class implements the `ConcurrentMap` interface and adds some more methods to the ones defined in the interface. This class supports the following:

- Full concurrency of read operations
- High expected concurrency for insert and delete operations

Both the elements (class and interface) were introduced in Java version 5, but in version 8, a lot of new methods similar to the ones provided by the stream API were developed.

In this recipe, you will learn how to use the `ConcurrentHashMap` class in your application and the most important methods it provides.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. Create a class named `Operation` with three attributes: a `String` attribute named `user`, a `String` attribute named `operation`, and a `Date` attribute named `time`. Add the methods to get and set the values of the attributes. The code of this class is very simple, so it won’t be included here.
- 2. Create a class named `HashFiller`. Specify that it implements the `Runnable` interface:
- 3. Declare a private `ConcurrentHashMap` attribute named `userHash`. The key of the hash will be a `String` type and its value will be a `ConcurrentLinkedDeque` object of `Operation` objects. Implement the constructor of the class to initialize the attribute:
- 4. Implement the `run()` method. We’re going to fill `ConcurrentHashMap` with 100 random `Operation` objects. First, generate random data and then use the `addOperationToHash()` method to insert the object in the hash:

```
public class HashFiller implements Runnable {
```

```
private ConcurrentHashMap<String, ConcurrentLinkedDeque<Operation>>
    userHash;

public HashFiller(ConcurrentHashMap<String, ConcurrentLinkedDeque
    <Operation>> userHash) {
    this.userHash = userHash;
}
```

```
@Override
public void run() {

    Random randomGenerator = new Random();
    for (int i = 0; i < 100; i++) {
        Operation operation = new Operation();
        String user = "USER" + randomGenerator.nextInt(100);
        operation.setUser(user);
        String action = "OP" + randomGenerator.nextInt(10);
        operation.setOperation(action);
        operation.setTime(new Date());

        addOperationToHash(userHash, operation);
    }
}
```

5. Implement the `addOperationToHash()` method. It receives the hash and the operation you want to add as parameters. The key in the map will be the user assigned to the operation. We use the `computeIfAbsent()` method to obtain the `ConcurrentLinkedDeque` object associated with the key. If the key exists, this method returns the value associated with it. If it doesn't, it executes the lambda expression passed as a parameter to this method to generate the value and associate it with the key. In this case, we generate a new `ConcurrentLinkedDeque` object. Finally, insert the operation to the deque:

```
```java
private void addOperationToHash(ConcurrentHashMap<String,
                                  ConcurrentLinkedDeque<Operation>>
                                  userHash, Operation operation) {

    ConcurrentLinkedDeque<Operation> opList = userHash
        .computeIfAbsent(operation.getUser(),
            user -> new ConcurrentLinkedDeque<>());

    opList.add(operation);
}
```
```

6. Now implement the `Main` class and include the `main()` method. First, declare a `ConcurrentHashMap` object and a `HashFiller` task:

```
```java
ConcurrentHashMap<String, ConcurrentLinkedDeque<Operation>>
    userHash = new ConcurrentHashMap<>();
HashFiller hashFiller = new HashFiller(userHash);
```
```

7. Execute 10 threads with the `HashFiller` class and wait for their finalization using the `join()` method:

```
```java
Thread[] threads = new Thread[10];
for (int i = 0; i < 10; i++) {
    threads[i] = new Thread(hashFiller);
    threads[i].start();
}

for (int i = 0; i < 10; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```
```
