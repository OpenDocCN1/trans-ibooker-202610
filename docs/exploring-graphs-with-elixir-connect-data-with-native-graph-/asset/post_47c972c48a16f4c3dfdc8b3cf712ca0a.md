
collector to each bucket. In other words, the values collected into buckets can go through more classification and collection downstream, as we'll see next.

Continuing with the previous example, instead of creating a map of all Person objects by age, let's get only people's names, grouped by age.

```
compare/fpij/OlderThan20.java

Map<Integer, List<String>> nameOfPeopleByAge =
people.stream()
.collect(
groupingBy(Person::getAge, mapping(Person::getName, toList())));
System.out.println("People grouped by age: " + nameOfPeopleByAge);
```

In this version, groupingBy takes two parameters: the first is the age, which is the criteria to group by, and the second is a Collector, which is the result of a call to the mapping function. These methods are from the Collectors utility class, statically imported for use in this code. The mapping method takes two details, the property on which to map (name in this case) and the type of the object to collect into, such as a list or set. Let's look at the output from this code:

```
People grouped by age: {35=[Greg], 20=[John], 21=[Sara, Jane]}
```

We see that the list of names is grouped by age.

Let's look at one more combination. Let's group the names by their first character and then get the oldest person in each group.

```
compare/fpij/OlderThan20.java

Comparator<Person> byAge = Comparator.comparing(Person::getAge);
Map<Character, Optional<Person>> oldestPersonOfEachLetter =
people.stream()
.collect(groupingBy(person -> person.getName().charAt(0),
        reducing(BinaryOperator.maxBy(byAge))));
System.out.println("Oldest person of each letter:");
System.out.println(oldestPersonOfEachLetter);
```

We first group the names based on their first letter. For this, we pass a lambda expression as the first parameter to the groupingBy method. From within this lambda expression, we return the first character of the name for grouping purposes. The second parameter in this example, instead of mapping, performs a reduce operation. In each group, it reduces the elements to the oldest person, as decided by the maxBy method. The syntax is a bit dense due to the combination of operations, but it reads like this: group by the first character of the name and reduce to the person with the maximum age. Let's look at the output, which lists the oldest person in each grouping of names that start with a given letter.

```
Oldest person of each letter:
{S=Optional[Sara - 21], G=Optional[Greg - 35], J=Optional[Jane - 21]}
```

We've seen the power of the collect method and the Collectors. Take a few minutes to examine the Collectors utility class in your integrated development environment or the documentation and get familiar with the facilities it offers. Next, we'll use lambda expressions to stand in for some filters.

## Listing All Files in a Directory

It's pretty simple to use the File class's list method to list all filenames in a directory. To get all the files instead of just their names, we can use the listFiles method. That's easy, but the challenge is how to proceed once we get the list. Rather than the long-winded traditional external iterator, we can use the elegant functional-style facility to iterate through the list. To achieve this, we have to reach out to the JDK's CloseableStream interface, along with some related higher-order convenience functions.

Here's the code to list the names of all the files in the current directory.

```
compare/fpij/ListFiles.java

Files.list(Paths.get("."))
.forEach(System.out::println);
```

To list files in a different directory, we can replace "." with the full path of the directory we desire.

We first created a Path instance from the string using the get method of the Paths convenience class. Then, using the list method of the Files utility class (in the java.nio.file package), we got a CloseableStream to iterate over the files in the given path. We then used the internal iterator, forEach, on it to print the filenames.

Let's look at part of the output from this code: listing the files and subdirectories of the current directory.

```
./aSampleFiles.txt
./bin
./fpij
...
```

If we want only the subdirectories in the current directory instead of a listing of all the files, we can use the filter method:

```
compare/fpij/ListDirs.java

Files.list(Paths.get("."))
.filter(Files::isDirectory)
.forEach(System.out::println);
```

The filter method extracted only the directories from the stream of files. Instead of passing in a lambda expression, we provided a method reference to the Files class's isDirectory method. Recall that the filter method expects a Predicate, which returns a boolean result, so this method fits the bill. Finally, we used the internal iterator to print the names of the directories. The output from this code will show the subdirectories of the current directory.

```
./.bin
./fpij
./output
...
```

That was simple and took fewer lines than it would have with old-style Java code. Next, let's look at listing only select files that match a pattern.

## Listing Select Files in a Directory

Java has long provided a variation of the list method to cherry-pick filenames. This version of list takes a FilenameFilter as its parameter. This interface has one method, accept, that takes two parameters: File dir (representing the directory) and String name (representing a filename). We'd return a true from the accept method to include the given filename in the list, and a false otherwise. Let's explore the options to implement this method.

It's a habitual practice in Java to pass to the list method an instance of an anonymous inner class that implements the FilenameFilter interface. For example, let's look at how we'd select only the java files in a fpij directory using that approach.

```
compare/fpij/ListSelectFiles.java

final String[] files =
    new File("fpij").list(new java.io.FilenameFilter() {
        public boolean accept(final File dir, final String name) {
            return name.endsWith(".java");
        }
    });
if(files != null) {
    for(String file: files) {
        System.out.println(file);
    }
}
```

That took some effort and a few lines of code. There's a lot of noise in that code: an object creation, a function call, an anonymous inner class definition, the embedded method within that class, and so on. We don't have to endure that pain anymore; we can simply pass a lambda expression that takes two parameters and returns a boolean result. The Java compiler can take care of the rest for us.

While we could simply replace the anonymous inner class with a lambda expression in the previous example, we can do better than that. The DirectoryStream facility can help traverse large directory structures more efficiently, so let's explore that route. There's a variation of the newDirectoryStream method, which takes an additional filter parameter.

Let's use lambda expressions to get a list of all java files in the fpij directory.

```
compare/fpij/ListSelectFiles.java

Files.newDirectoryStream(
    Paths.get("fpij"), path -> path.toString().endsWith(".java"))
    .forEach(System.out::println);
```

We got rid of the anonymous inner class and turned the verbose code into short and sweet code. The net effect of the two versions is the same. Let's print the selected files.

The code will display only the .java files in the mentioned directory, as in this partial output:

```
fpij/Compare.java
fpij/IterateString.java
fpij/ListDirs.java
...
```

We picked files based on filenames, but we can easily pick files based on file properties, such as whether a file is executable, readable, or writable. For this, we need a variation of the listFiles method that takes FileFilter as its parameter. Once again, we can use lambda expressions instead of creating an anonymous inner class. Let's look at an example of listing all hidden files in the current directory.

```
compare/fpij/ListHiddenFiles.java

final File[] files = new File(".").listFiles(file -> file.isHidden());
```

If we're working with a large directory, then we can use the DirectoryStream instead of directly using the methods on File.

The signature of the lambda expression we passed to the listFiles method conforms to the signature of the FileFilter interface's accept method. In the lambda expression, we receive a File instance as the parameter, named file in this example. We return a boolean true if the file has the hidden property, or a false otherwise.

We can further reduce the code here; rather than passing a lambda expression, we can use a method reference to make the code more concise:

```
compare/fpij/ListHiddenFiles.java

new File(".").listFiles(File::isHidden);
```

We created the lambda-expressions version and then refactored it to the more concise method-references version. When working on new code, it's perfectly OK to take that route. If we can see the concise code from miles away, then of course we can readily key that in. In the spirit of “make it work, then make it better,” it's good to get something simple working first, and once we understand the code, we can take the next step to refactor for conciseness, performance, and so on.

We worked through an example to filter out select files from a directory. Next, we'll look at how to explore subdirectories of a
