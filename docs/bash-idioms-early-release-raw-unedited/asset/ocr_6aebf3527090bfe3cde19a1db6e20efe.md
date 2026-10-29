# Chapter 2. Looping Lingo

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 2nd chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the authors at bash-idioms@jpsdomain.org.

It’s not just C-style `for` loops—bash includes other syntax and styles; some are more familiar to Python programmers, but each has its place. There is a `for` loop with no apparent arguments, useful in both scripts and inside functions. There is an iterator-like `for` loop with explicit values and values that can come from other commands.

# Looping constructs

Looping constructs are common in programming languages. Since the invention of C language many programming languages have adopted the C-style “for” loop. It’s such a powerful, readable construct because it groups the initialization code, the termination condition and the iteration code all into one place. For example, in C (or Java, or ...):