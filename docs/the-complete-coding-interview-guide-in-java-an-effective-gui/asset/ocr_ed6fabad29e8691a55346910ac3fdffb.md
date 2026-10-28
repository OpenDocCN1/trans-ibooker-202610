| SYMBOL | VALUE |
|--------|-------|
| I | 1 |
| IV | 4 |
| V | 5 |
| IX | 9 |
| X | 10 |
| XL | 40 |
| L | 50 |
| XC | 90 |
| C | 100 |
| CD | 400 |
| D | 500 |
| CM | 900 |
| M | 1000 |

Figure 15.1 – Roman numbers

**Solution**: This problem relies on the fact that Roman numbers are common knowledge. If you've never heard about Roman numbers, then it is advisable to mention this to the interviewer. They will probably agree to give you another coding challenge in place of this one. But if you do know what the Roman numbers are, then great – let's see how we can write an application that solves this problem.

The algorithm for this problem can be deduced from several examples. Let's look at several use cases:

+   - *n* = 73 = 50+10+10+1+1+1 = L+X+X+I+I+I = LXXIII
+   - *n* = 558 = 500+50+5+1+1+1 = D+L+V+I+I+I = DLVIII
+   - *n* = 145 = 100+(50-10)+5 = C+(L-X)+V = C+XL+V = CXLV
+   - *n* = 34 = 10+10+10+(5-1) = X+X+X+(V-I) = X+X+X+IV = XXXIV
+   - *n* = 49 = (50-10)+(10-1) = (L-X)+(X-I) = XL+IX = XLIX

Roughly, we take the given number and try to find the Roman symbols corresponding to ones, tens, hundreds, or thousands. This algorithm can be expressed as follows:

+   1. Start from the thousand's place and print the corresponding Roman number. For example, if the digit at thousand's place is 4, then print the Roman equivalent of 4,000, which is MMMM.
+   2. Continue by dividing the number using digits at hundred's place and print the corresponding Roman number.