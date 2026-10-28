Authors of computer science textbooks typically include collections among data structures. With these three collections mentioned above, you can do a surprising amount of coding. Sometimes you need specialized functionality such as an ordered list that contains no duplicates. Most programming libraries give you access to a broad range of such structures and allow you to create your own.

C. S. Lewis wrote many other books and novels. Another dictionary we could create might use decades like “1930s”, “1940s”, “1950s”, and “1960s” as the keys. The values could be the list of titles in chronological order published within each ten-year span.

Collections can be parts of other collections. For example, I can represent a table of company earnings as a list of lists of numbers and strings. However, some languages require the contained objects all to be of the same type. For example, maybe a list needs to be all integers or all strings. Other languages, including Python, allow you to store different kinds of objects.

# Exercise 1.4

In the section on functions, we looked at **maximum** with two parameters. Think about and describe in words how a similar function that took a single parameter, a list of numbers, might work.

An object which we can change is called *mutable*. The list of books is mutable while an author is writing and publishing. Once the author passes away, we might lock the collection and declare it *immutable*. No one can make any further changes. For dictionaries, the keys usually need to be immutable and are frequently strings of text or numbers.

# Exercise 1.5

Is a string a collection? If so, should it be mutable or immutable?

# 1.6 Conditional processing

When you write code, you create instructions for a computer that state what steps the computer must take to accomplish some task. In that sense, you are specifying a recipe. Depending on some conditions, like if a bank balance is positive or a car’s speed exceeds a stated limit, the sequence of steps may vary. We call how your code moves from one step to another its *flow*.