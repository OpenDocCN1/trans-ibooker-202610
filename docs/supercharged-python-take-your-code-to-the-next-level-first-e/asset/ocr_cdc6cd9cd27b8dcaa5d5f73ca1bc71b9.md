| 100 | 2 |
| --- | - |
| 1000 | 3 |
| 3162.277 | 3.5 (approx.) |
| 10000 | 4 |
| 31622.777 | 4.5 (approx.) |

Remember that some of the results in Table 11.7 are approximate. For example, if you take the base-10 logarithm of 31,622.777, you’ll get a number very close to 4.5.

# ## 11.13.2 Applying a Logarithm to a Practical Problem

Now let’s return to the number guessing game. If you play the game a few times, you should see there’s an obvious strategy for getting the answer in less than N guesses, where N is the size of the range. The worst strategy would be to start with 1, then go one higher, guessing 2, and so on, until you’ve covered the whole range.

On average, that strategy would take N/2 guesses to succeed: 25 guesses if the range is 1 to 50. But with the right strategy, you should be able to do much better than that, raising the following question.