3. To disable compile-time checks, wrap the statement in an unchecked block, write the value of y to the console, decrement it, and repeat, as shown in the following code:

```
unchecked
{
    int y = int.MaxValue + 1;
    WriteLine($"Initial value: {y}");
    y--;
    WriteLine($"After decrementing: {y}");
    y--;
    WriteLine($"After decrementing: {y}");
}
```

4. Run the code and view the results, as shown in the following output:

```
Initial value: -2147483648
After decrementing: 2147483647
After decrementing: 2147483646
```

Of course, it would be rare that you would want to explicitly switch off a check like this because it allows an overflow to occur. But perhaps you can think of a scenario where you might want that behavior.

# Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore with deeper research into this chapter's topics.

# Exercise 3.1 – Test your knowledge

Answer the following questions:

+   1. What happens when you divide an int variable by 0?
+   2. What happens when you divide a double variable by 0?
+   3. What happens when you overflow an int variable, that is, set it to a value beyond its range?
+   4. What is the difference between x = y++; and x = ++y;?
+   5. What is the difference between break, continue, and return when used inside a loop statement?
+   6. What are the three parts of a for statement and which of them are required?
+   7. What is the difference between the = and == operators?