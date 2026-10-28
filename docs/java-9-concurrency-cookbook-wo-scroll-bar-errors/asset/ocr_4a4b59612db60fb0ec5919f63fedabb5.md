```
personsBySalary.keySet().forEach(key -> {
    List<Person> listOfPersons = personsBySalary.get(key);
    System.out.printf("%s: %d \n", key, listOfPersons.size());
});
```

8. Then, we'll implement a collect operation that will generate another Map. In this case, the keys will be the first name of the persons and the value will be the last names of the people with the same first name concatenated in one String. To implement this behavior, we have use the collect() method of the Stream class and the toConcurrentMap() method of the Collectors class. We pass as parameters to that method a lambda expression to obtain the key, a lambda expression to obtain the value, and a lambda expression to resolve the situations where the key exists in the final Map. Then, we use the forEach() method to process all the keys and write its associated values:

```
ConcurrentMap<String, String> nameMap = persons
    .parallelStream().collect(Collectors
    .toConcurrentMap(p -> p.getFirstName(),
                    p -> p.getLastName(),
                    (s1, s2) -> s1 + ", " + s2));
nameMap.forEach((key, value) -> {
    System.out.printf("%s: %s \n", key, value);
});
```

9. Until now, in all the examples of the collect() method we have implemented, we used the version of that method that receives an implementation of the Collector interface. But there's another version of the collect() method. With this version of the collect() method, we will implement a collect operation that generates a List with the persons who have a salary greater than 50,000. We pass to the collect() method an expression to create the List (the List::new method), a lambda expression to process a list and an element of the stream, and an expression to process the two lists (the List::addAll method):

```
List<Person> highSalaryPeople = persons
    .parallelStream().collect(
        ArrayList::new, (list, person) -> {
            if (person.getSalary() > 50000) {
                list.add(person);
            }
        },
        ArrayList::addAll
    );
System.out.printf("High Salary People: %d\n",
                  highSalaryPeople.size());
```