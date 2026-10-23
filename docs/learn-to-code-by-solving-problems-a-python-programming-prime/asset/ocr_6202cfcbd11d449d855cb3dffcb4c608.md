than or equal to 16. The second while loop will set right to 6, the leftmost index whose position is greater than 21. Subtracting left from right, we obtain 6 − 4 = 2, which means that there are two triples involving positions 6 and 11. Before continuing, I encourage you to convince yourself that these while loops work just fine in “special” cases, such as when there are no suitable third positions or when there is one suitable third position.

We’ve made strong progress in this section. Our code here is certainly more efficient than the code we gave in Listing 9-5. However, it still isn’t efficient enough. If you submit to the judge, you’ll see that it doesn’t get much further than we got last time. It still times out on most of the test cases.

# Efficiency of Our Program

The problem with our program is that finding the third position can still take a long time. Those while loops still have some inefficiency. I can demonstrate this with a new list of positions, namely, the positions from 1 to 32.

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,
17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32]

Let’s focus on when i is 0 and j is 7; these are the positions 1 and 8. For the third position, we’re looking for positions that are greater than or equal to 15 and less than or equal to 22. To find the 15, the first while loop scans to the right, one position at a time. It scans the 9, then the 10, then the 11, then the 12, then the 13, then the 14, and finally the 15. Then the second while loop takes over, doing a similarly large amount of scanning, one position at a time, all the way until it finds the 23.

Each while loop implements what’s known as a linear search. A linear search is a technique that searches through a collection one value at a time. It’s a lot of work, scanning through all those values! And there are many other values of i and j that lead to a similar amount of work. For example, try tracing what happens when i is 0 and j is 8, or when i is 1 and j is 11.

How can we improve on this? How can we avoid scanning through a huge chunk of the list, looking for the appropriate left and right indices?

Suppose I give you a book with a thousand sorted integers, one integer per line. I ask you to find me the first integer that’s greater or