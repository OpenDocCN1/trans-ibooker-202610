# Summing Values

By now I'm sure you've recognized that Collectors, with so many functions, may be one of the most comprehensive utility classes in the JDK. If you're looking for an intellectual company to hang out with on a Friday evening, I suggest you invite the Collectors—you won't be disappointed. In addition to all the methods we've seen, the Collectors utility class also has a function to sum values. Let's use that to total the number of email addresses a person has, while grouping the persons in a collection based on their last name.

```
```java
var namesAndEmailAddressesCount =
SAMPLE_DATA.stream()
.collect(groupingBy(
Person::lastName,
summingInt(person -> person.emailAddresses().size())));
```
```

```
```java
System.out.println(namesAndEmailAddressesCount);
```
```

As we've seen before, the first argument to the groupingBy decides the label for the group. The second argument decides what gets stored for each group. In this case, it is the sum of all the numbers of email addresses that it sees for each group. Let's confirm that by viewing the output:

```
```
{Xie=3, Baker=2, Smith=1, Walker=1, Shah=2, Doe=0, Lee=2}
```
```

If it gets overwhelming, you're not alone, the Collectors utility class relentlessly keeps on giving. Take a break and then continue with the next couple of examples in this chapter.