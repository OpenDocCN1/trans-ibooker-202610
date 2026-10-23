+   - 14, 16, 18
+   - 14, 18, 23
+   - 1, 6, 16
+   - 1, 6, 14
+   - 1, 6, 11
+   - 1, 11, 23
+   - 6, 14, 23
+   - 6, 11, 16
+   - 6, 11, 18
+   - 11, 16, 23
+   - 11, 14, 18

Good news: our program outputs 11 for this test case! It does so because it eventually finds each satisfying triple. For example, at some point, position1 will be 14, position2 will be 16, and position3 will be 18. That triple satisfies the distance requirements, so our program will count it in our total. Don’t be worried about what will happen, later, when position1 is 18, position2 is 16, and position3 is 14. We definitely don’t want to count that one, because these throws are not going to the right. We’re fine, though: the if statement ④ prevents these triples from being processed.

Our program is correct. But as you’ll see if you submit it to the judge, it is not efficient enough. For this problem, and many competitive programming problems, the first few test cases are small—just a few cows, a few lifeguards, or a few ski hills. Our program should be able to solve those in time. The remaining test cases test our program closer and closer to the limit of acceptable input. Our program does not solve those in time. It’s too slow.

# Efficiency of Our Program

To understand why our program is so slow, it helps to think about the number of triples that it must go through. Think back to the test case we just studied, which had seven cows. How many triples will our program check? Well, for the first cow, there are seven choices: 16, 14, 23, and so on. There are also seven choices for the second cow, and seven choices for the third cow. Multiplying these together, we see that our program checks 7 * 7 * 7 = 343 triples.