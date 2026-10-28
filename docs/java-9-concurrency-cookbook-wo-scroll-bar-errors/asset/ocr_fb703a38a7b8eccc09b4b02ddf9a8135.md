3. Then, create a class named PersonGenerator to create a random list of Person objects. Implement a static method named generatePersonList() in this class, which receives the number of persons you want to generate and returns a List<Person> object with the number of persons. Here, we include a version of this method, but feel free to change it:

```
public class PersonGenerator {

    public static List<Person> generatePersonList (int size) {
        List<Person> ret = new ArrayList<>();

        String firstNames[] = {"Mary","Patricia","Linda",
            "Barbara","Elizabeth","James",
            "John","Robert","Michael",
            "William"};
        String lastNames[] = {"Smith","Jones","Taylor",
            "Williams","Brown","Davies",
            "Evans","Wilson","Thomas",
            "Roberts"};

        Random randomGenerator=new Random();
        for (int i=0; i<size; i++) {
            Person person=new Person();
            person.setId(i);
            person.setFirstName(firstNames[randomGenerator
                .nextInt(10)]);
            person.setLastName(lastNames[randomGenerator
                .nextInt(10)]);
            person.setSalary(randomGenerator.nextInt(100000));
            person.setCoefecient(randomGenerator.nextDouble()*10);
            Calendar calendar=Calendar.getInstance();
            calendar.add(Calendar.YEAR, -randomGenerator
                .nextInt(30));
            Date birthDate=calendar.getTime();
            person.setBirthDate(birthDate);
            ret.add(person);
        }
        return ret;
    }
}
```

4. Now, create a class named MySupplier and specify that it implements the Supplier interface parameterized with the String class:

```
public class MySupplier implements Supplier<String> {
```