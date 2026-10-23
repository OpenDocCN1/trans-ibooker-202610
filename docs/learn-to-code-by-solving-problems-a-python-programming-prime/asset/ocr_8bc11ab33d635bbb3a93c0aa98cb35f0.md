Now here’s a test case that can be solved, but not by our program:

```
```text
6 10
2 3
2 4
3 4
2 5
3 5
4 5
1 6
3 6
4 6
5 6
```
```

The mistake that our program makes is to use grass type 1 for pasture 1. In so doing, it’s forced to use grass type 5—which isn’t allowed!—for pasture 6. Our program fails, but don’t conclude that there isn’t a way to solve this test case. In particular, use grass type 2 for pasture 1, and you should be able to find a way to solve this test case using only four grass types. It’s possible to solve these kinds of test cases with a more sophisticated program, and I encourage you to think about this on your own if you’re interested.

# Summary

In this chapter, we learned how to open, read from, write to, and close files. Files are useful whenever you need to store information and use it as input later. They’re also useful for communicating information to your users. We also learned that we process files similarly to how we process standard input and standard output.

In the next chapter, we’ll learn how to store a collection of values in a Python set or dictionary. Storing a collection of values—that sounds like what a list does. We’ll see, though, that sets and dictionaries can make it easier for us to solve some kinds of problems.

# Chapter Exercises

Here are some exercises for you to try. All of them are from the USACO judge and require reading and writing files. They’ll also require you to