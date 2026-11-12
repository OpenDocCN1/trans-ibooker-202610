
The Stream API can be readily used to process a collection of data in memory. Next, we'll see how to deal with data that's in a file.

## Refactoring File Processing

Reading data from a file is a common operation in programming and I'm sure you've done that countless number of times. But the code to perform that operation was rather verbose and messy in the past. Let's take a look at a function that reads the contents of a file and counts the number of occurrences of a word in it. Then we'll refactor to use the functional style.

Let's start with a small set of tests to verify that a `countInFile` function works as expected:

```
refactoring/fpij/WordCountTest.java

public class WordCountTest {
    WordCount wordCount;

    @BeforeEach
    public void init() {
        wordCount = new WordCount();
    }

    @Test
    public void count() {
        assertAll(
            () -> assertEquals(2,
                wordCount.countInFile("public", "fpij/WordCount.java")),
            () -> assertEquals(1,
                wordCount.countInFile("package", "fpij/WordCount.java"))
        );
    }
}
```

The tests illustrate that the method under test takes a search word and the path to the file to search in. Here's the imperative style code that's crying to be refactored:

```
refactoring/fpij/WordCount.java

```java
public class WordCount {
    public long countInFile(
        String searchWord, String filePath) throws IOException {
        long count = 0;

        BufferedReader bufferedReader =
            new BufferedReader(new FileReader(filePath));

        String line = null;

        while ((line = bufferedReader.readLine()) != null) {
            String[] words = line.split(" ");

            for (String word : words) {
                if (word.equals(searchWord)) {
                    count++;
                }
            }
        }

        return count;
    }
}
```
```

The function checks if there are more lines to read from the file by checking the content read against null—eek, that's rather unsightly. We need easier ways to read and process the contents of files and thankfully the JDK has newer functions that bring forward the charm of functional style for that.

Pause, refactor, and continue.

Given the contents of a file, we want to look for the number of occurrences of a word in it. We can use the filter operation to check if each word is what we're looking for, followed by a reduce operation to count the number of appearances. But, to take those steps, we need a stream of the contents of the file.

The **Files** class in the JDK's `java.nio.file` package has the right function for it. The `lines` method returns a `Stream<String>`, and we can apply our favorite **Stream** operations to process the contents of the file. Let's use that function to refactor the `countInFile` method:

```
public long countInFile(
    String searchWord, String filePath) throws IOException {
    return Files.lines(Paths.get(filePath))
        .flatMap(line -> Stream.of(line.split(" ")))
        .filter(word -> word.equals(searchWord))
        .count();
}
```

The `lines` method creates an internal iterator to work with one line of file contents at a time. But we need to work with the words in the file and not just the lines. We can call `split` to break a line into words so that we can perform further processing on the words. Since splitting a line into words is a one-to-many mapping, we have to use `flatMap` instead of `map` to transform the data from a stream of lines to a stream of words —see [When to Use map vs. flatMap]. Once we get the stream of words, we can `filter` and then finally count.

In addition to concise and expressive code, as a bonus, we don't have to mess with `null` and that's a big win in itself.

Next, we'll work with an example where the data is in a `Map`.

## Refactoring Data Grouping Operations

Grouping data based on some criteria is a common operation in business applications. We may want to group employees based on their work location, projects based on their business units, products based on their latest revenues, and so on. Creating a Map for the group and adding values for its keys can get verbose in the imperative style and may also involve some inefficiencies. You'll see a stark contrast between the imperative style version of code that does such processing and its equivalent functional style code.

To gain insight into refactoring code that does grouping operations, we'll take a collection of hypothetical scores from a game, provided as key-value pairs of player names and scores. Let's start with some tests that verify that a function namesForScores groups the data based on the scores and creates a list of names associated with for each store.

```
refactoring/fpij/ScoresTest.java

public class ScoresTest {
    Scores scores;

    @BeforeEach
    public void init() {
        scores = new Scores();
    }

    @Test
    public void namesForScores() {
        assertAll(
            () -> assertEquals(Map.of(), scores.namesForScores(Map.of())),
            () -> assertEquals(
                Map.of(1, Set.of("Jill")), scores.namesForScores(Map.of("Jill", 1))),
            () -> assertEquals(
                Map.of(1, Set.of("Jill"), 2, Set.of("Paul")),
                scores.namesForScores(Map.of("Jill", 1, "Paul", 2))),
            () -> assertEquals(
                Map.of(1, Set.of("Jill", "Kate"), 2, Set.of("Paul")),
                scores.namesForScores(Map.of("Jill", 1, "Paul", 2, "Kate", 1)))
        );
    }
}
```

To group the data based on the scores, we'll have to iterate over the given Map's key set. For each player, we check if their score value is already in the destination map. If it is, we add the name to the existing key's value set. If the score isn't already present as a key, then we create a new set with the name of the player and add the set as a value for the new key in the destination map. Here's the code that does those mundane operations:

```
refactoring/fpij/Scores.java

public class Scores {
    public Map<Integer, Set<String>> namesForScores(
        Map<String, Integer> scores) {

        Map<Integer, Set<String>> namesForScores = new HashMap<>();

        for (String name : scores.keySet()) {
            int score = scores.get(name);

            Set<String> names = new HashSet<>();
            if (namesForScores.containsKey(score)) {
                names = namesForScores.get(score);
            }

            names.add(name);
            namesForScores.put(score, names);
        }

        return namesForScores;
    }
}
```

Lots of garbage variables, explicit mutation, low-level operations, and the list goes on—shudder.

Pause, refactor, and continue.

The entire body of the namesForScores method can be refactored to literally three lines of code:

```
public Map<Integer, Set<String>> namesForScores(
    Map<String, Integer> scores) {
    return scores.keySet()
        .stream()
        .collect(groupingBy(scores::get, toSet()));
}
```

We iterate over the key set using the stream method and ask the values to be grouped based on the value of the score for each name. We then ask the names to be placed into a set, by providing the Collector returned by toSet to the groupingBy function. The code is shockingly potent.

Through each example, we've worked with increasing complexity, and each example has given a few new ideas to consider when refactoring to functional style. The next example, the last one in this refactoring series of tasks, has a nice added complexity and brings along a powerful set of solutions—let's take a look.

## Refactoring Nested Loops

The imperative style code you see in this section was sent to me by a developer who was interested in applying functional programming and was curious to learn how to refactor it into the functional style. The code creates Pythagorean triples[19] of positive numbers (a, b, c) which satisfy the condition a² + b² = c².

Let's start with a few tests to verify the results of the compute method of a PythagoreanTriples class:

```
refactoring/fpij/PythagoreanTriplesTest.java

public class PythagoreanTriplesTest {
    PythagoreanTriples pythagoreanTriples;

    @BeforeEach
    public void init() {
        pythagoreanTriples = new PythagoreanTriples();
    }

    @Test
    public void compute() {
        assertAll(
            () -> assertEquals(List.of(), pythagoreanTriples.compute(0)),
            () -> assertEquals(List.of(triple(3, 4, 5)),
                pythagoreanTriples.compute(1)),
            () -> assertEquals(
                List.of(triple(3, 4, 5), triple(8, 6, 10), triple(5, 12, 13)),
                pythagoreanTriples.compute(3)),
            () -> assertEquals(
                List.of(triple(3, 4, 5), triple(8, 6, 10),
                    triple(5, 12, 13), triple(15, 8, 17),
                    triple(12, 16, 20)),
                pythagoreanTriples.compute(5))
        );
    }
}
```
