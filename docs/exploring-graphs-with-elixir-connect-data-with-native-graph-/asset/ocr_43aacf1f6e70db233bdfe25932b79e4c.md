The test makes use of a Person which can be implemented as a record (or a class in older versions of Java).

```
refactoring/fpij/Person.java

public record Person(String name, int age) {}
```

An agency is responsible for deciding if a chaperone is required for trips taken by a group of people. The logic to make that decision is in the Agency class’s isChaperoneRequired method:

```
refactoring/fpij/Agency.java

public class Agency {
    public boolean isChaperoneRequired(Set<Person> people) {
        boolean required = true;

        if(people.size() == 0) {
            required = false;
        } else {
            for(var person: people) {
                if(person.age() >= 18) {
                    required = false;
                    break;
                }
            }
        }

        return required;
    }
}
```

What the method does is rather simple, but it does it with a lot of noise. If anyone in the group is 18 years or older or the group is empty, then no chaperone is needed. This code can use some refactoring to reduce clutter and remove mutability.

Pause, refactor, and continue.