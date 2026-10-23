B.

6
5
4

C.

5
4

D.

5
4
3

E.

6
5
4
3

Answer: C. Many while loops do something and then update the loop variable, but not this one. This one first decrements the loop variable x and then outputs it. As 6 is greater than 4, an iteration of the loop runs, which assigns 5 to x and then outputs 5. Next, 5 is greater than 4, so we have another iteration, this one assigning 4 to x and outputting 4. And that’s it: 4 is not greater than 4, so the loop terminates.

# Nesting Loops in Loops

We can nest loops inside of while loops, much as we can nest loops inside of for loops. In “Nesting” in Chapter 3, I noted that the inner for loop