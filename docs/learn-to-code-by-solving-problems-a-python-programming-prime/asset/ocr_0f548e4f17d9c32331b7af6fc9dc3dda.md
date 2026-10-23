# Solving the Problem

To solve Slot Machines, we know that we need a while loop, not a for loop, because we can’t predict the number of iterations in advance. Each iteration of the loop will play the current slot machine. When the loop terminates, Martha will have no quarters left, and we will output the number of times she played.

Here’s what we need to do on each iteration:

+   - Decrease Martha’s quarters by one (since it costs one quarter to play a slot machine).
+   - If Martha is currently on the first slot machine, play that machine. This involves increasing the number of times this machine has been played. If this is the 35th play, then pay Martha and reset the number of times that this machine has been played to 0.
+   - If Martha is currently on the second slot machine, play that machine (similar to how we played the first machine).
+   - If Martha is currently on the third slot machine, play that machine (similar to how we played the first machine).
+   - Increase the number of Martha’s plays (since we just played a machine).
+   - Move to the next machine. If Martha just played the first machine, we want to move to the second; if she just played the second, we want to move to the third; and if she just played the third, we want to cycle back to the first.

Our programs are getting longer now, so outlining the plan as I just did is a useful technique for keeping the complexity under control and guiding us toward correct code. We can use the outline to make sure we’re following the plan and not forgetting anything.

Our code is in Listing 4-1.

```
quarters = int(input())
first = int(input())
second = int(input())
third = int(input())

plays = 0
① machine = 0

② while quarters >= 1:
```