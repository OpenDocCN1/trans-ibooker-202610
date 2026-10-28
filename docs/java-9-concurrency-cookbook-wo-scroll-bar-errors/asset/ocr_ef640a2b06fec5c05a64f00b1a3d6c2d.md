5. Declare a private AtomicInteger attribute named counter and initialize it in the constructor of the class:

```
private final AtomicInteger counter;
public MySupplier() {
    counter=new AtomicInteger(0);
}
```

6. Implement the get() method defined in the Supplier interface. This method will return the next element of the stream:

```
@Override
public String get() {
    int value=counter.getAndAdd(1);
    return "String "+value;
}
```

7. Now, create a class named Main and implement the main() method in it:

```
public class Main {
    public static void main(String[] args) {
```

8. First, we'll create a Stream object from a list of elements. Create the PersonGenerator class to create a list of 10,000 Person objects, and use the parallelStream() method of the List object to create the Stream. Then, use the count() method of the Stream object to get the number of elements of the Stream:

```
System.out.printf("From a Collection:\n");
List<Person> persons=PersonGenerator.generatePersonList(10000);
Stream<Person> personStream=persons.parallelStream();
System.out.printf("Number of persons: %d\n", personStream.count();
```

9. Then, we'll create a Stream from a generator. Create an object of the MySupplier class. Then, use the static method generate() of the Stream class, passing the created object as a parameter to create the stream. Finally, use the parallel() method to convert the stream created to a parallel stream, the limit() method to get the first ten elements of the stream, and the forEach() method to print the elements of the stream: