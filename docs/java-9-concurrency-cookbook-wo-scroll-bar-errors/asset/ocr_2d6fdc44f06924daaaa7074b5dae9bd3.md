5. Then, we'll use the mapToDouble() method to convert the stream of Person objects into DoubleStream of double values. Create a parallel stream using the parallelStream() method and then use the mapToDouble() method, passing as parameter a lambda expression that receives a Person object and returns its salary, which is a double number. Then use the distinct() method to get the unique values and the forEach() method to write them to the console. We also get the number of different elements written using the count() method:

```
DoubleStream ds = persons.parallelStream().mapToDouble
    (p -> p.getSalary());
ds.distinct().forEach(d -> {
    System.out.printf("Salary: %f\n", d);
});
ds = persons.parallelStream().mapToDouble(p -> p.getSalary());
long size = ds.distinct().count();
System.out.printf("Size: %d\n", size);
```

6. Now, we'll transform the Person objects of the stream into BasicPerson objects. Create the stream using the parallelStream() method and use the map() method to transform the objects. This method receives as parameter a lambda expression that receives a Person object, creates a new BasicPerson object, and establishes the value of its attributes. Then, we write the values of the attributes of the BasicPerson object using the forEach() method:

```
List<BasicPerson> basicPersons = persons.parallelStream().map
    (p -> {
        BasicPerson bp = new BasicPerson();
        bp.setName(p.getFirstName() + " " + p.getLastName());
        bp.setAge(getAge(p.getBirthDate()));
        return bp;
    }).collect(Collectors.toList());

basicPersons.forEach(bp -> {
    System.out.printf("%s: %d\n", bp.getName(), bp.getAge());
});
```