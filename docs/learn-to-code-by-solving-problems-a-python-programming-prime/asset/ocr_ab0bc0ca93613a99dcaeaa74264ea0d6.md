The continue keyword is enticing because it seems to give us a way to get us out of an iteration that we don’t want to be in. “This isn’t a vowel. I’m out of here!” But an if statement can also be used to obtain the same behavior, and the logic is often clearer:

```
>>> s = 'zephyr'
>>> i = 0
>>> while i < len(s):
...   if s[i] in 'aeiou':
...     print(s[i], i)
...   i = i + 1
...
e 1
```

Rather than skip the iteration when the current character is not a vowel, the if statement processes it when it is a vowel.

# Summary

The unifying feature of the problems in this chapter is that we don’t know in advance how many iterations of a loop will be required.

**Slot Machines** The number of iterations depends on the initial number of quarters and the payouts of the slot machines.

**Song Playlist** The number of iterations depends on how many buttons were pressed.

**Secret Sentence** The number of iterations, and what to do on each iteration, depends on where vowels are located in a string.

When the number of iterations is unknown, we turn to the while loop, which runs as long as needed. Using a while loop is more error-prone than code that uses a for loop. It’s also more flexible, as we are freed from the for loop constraint of systematically looping through a sequence.

In the next chapter, we’ll learn about lists, which allow us to store large amounts of numeric or string data. And how do you suppose we’ll process all of that data? Yes: loops! Practice the following exercises to hone your loop skills. You’ll be using them a lot when we solve problems using lists.