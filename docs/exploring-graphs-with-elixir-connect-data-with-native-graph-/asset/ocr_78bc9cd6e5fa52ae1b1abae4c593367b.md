./fpij
...

If we want only the subdirectories in the current directory instead of a listing of all the files, we can use the filter method:

# compare/fpij/ListDirs.java

```
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