+   6. What is the ISO culture code for Welsh?
+   7. What is the difference between localization, globalization, and internationalization?
+   8. In a regular expression, what does $ mean?
+   9. In a regular expression, how can you represent digits?
+   10. Why should you not use the official standard for email addresses to create a regular expression to validate a user's email address?

# Exercise 8.2 – Practice regular expressions

In the Chapter08 solution/workspace, create a console application named Exercise02 that prompts the user to enter a regular expression and then prompts the user to enter some input and compare the two for a match until the user presses Esc, as shown in the following output:

```
The default regular expression checks for at least one digit.
Enter a regular expression (or press ENTER to use the default): ^[a-z]+$
Enter some input: apples
apples matches ^[a-z]+$? True
Press ESC to end or any key to try again.
Enter a regular expression (or press ENTER to use the default): ^[a-z]+$
Enter some input: abc123xyz
abc123xyz matches ^[a-z]+$? False
Press ESC to end or any key to try again.
```

# Exercise 8.3 – Practice writing extension methods

In the Chapter08 solution/workspace, create a class library named Exercise03 that defines extension methods that extend number types such as BigInteger and int with a method named ToWords that returns a string describing the number; for example, 18,000,000 would be eighteen million, and 18,456,002,032,011,000,007 would be eighteen quintillion, four hundred and fifty-six quadrillion, two trillion, thirty-two billion, eleven million, and seven.

You can read more about names for large numbers at the following link: https://en.wikipedia.org/wiki/Names_of_large_numbers

# Exercise 8.4 – Explore topics

Use the links on the following page to learn more detail about the topics covered in this chapter:

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-8---working-with-common-net-types