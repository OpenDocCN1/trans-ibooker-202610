If you enter en-US instead of en-GB, then you must enter the date using month/day/year.

6. Rerun the code and try a different culture, such as Danish in Denmark, as shown in the following output:

```
Enter an ISO culture code: da-DK
Enter your name: Mikkel
Enter your date of birth: 12/3/1980
Enter your salary: 340000
Mikkel was born on a onsdag, is 18.656.640 minutes old, and earns
340.000,00 kr.
```

In this example, only the date and salary are globalized into Danish. The rest of the text is hardcoded as English. This book does not currently include how to translate text from one language to another. If you would like me to include that in the next edition, please let me know.

**Good Practice:** Consider whether your application needs to be internationalized and plan for that before you start coding! Write down all the pieces of text in the user interface that will need to be localized. Think about all the data that will need to be globalized (date formats, number formats, and sorting text behavior).

# Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore with deeper research into the topics in this chapter.

# Exercise 8.1 – Test your knowledge

Use the web to answer the following questions:

+   1. What is the maximum number of characters that can be stored in a string variable?
+   2. When and why should you use a SecureString type?
+   3. When is it appropriate to use a StringBuilder class?
+   4. When should you use a LinkedList<T> class?
+   5. When should you use a SortedDictionary<T> class rather than a SortedList<T> class?