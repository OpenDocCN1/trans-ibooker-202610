# How to do it...

Follow these steps to implement the example:

+   1. Create a class named Person. This class will have six attributes to define some basic characteristics of a person. We will implement the methods to get() and set() the values of the attributes, but they won't be included here:

```
public class Person {
    private int id;
    private String firstName;
    private String lastName;
    private Date birthDate;
    private int salary;
    private double coeficient;
}
```

+   2. Now, implement a class named PersonGenerator. This class will only have a method named generatedPersonList() to generate a list of Person objects with random values with the size specified in parameters. This is the source code of this class:

```
public class PersonGenerator {
    public static List<Person> generatePersonList (int size) {
        List<Person> ret = new ArrayList<>();

        String firstNames[] = {"Mary","Patricia","Linda",
            "Barbara","Elizabeth","James",
            "John","Robert","Michael","William"};
        String lastNames[] = {"Smith","Jones","Taylor",
            "Williams","Brown","Davies",
            "Evans","Wilson","Thomas","Roberts"};

        Random randomGenerator=new Random();
        for (int i=0; i<size; i++) {
            Person person=new Person();
            person.setId(i);
            person.setFirstName(firstNames[randomGenerator.nextInt(10)]);
            person.setLastName(lastNames[randomGenerator.nextInt(10)]);
            person.setSalary(randomGenerator.nextInt(100000));
            person.setCoeficient(randomGenerator.nextDouble()*10);
            Calendar calendar=Calendar.getInstance();
            calendar.add(Calendar.YEAR, -randomGenerator.nextInt(30));
            Date birthDate=calendar.getTime();
```