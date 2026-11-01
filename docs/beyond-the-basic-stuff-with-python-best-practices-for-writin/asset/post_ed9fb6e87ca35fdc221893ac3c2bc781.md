
## 5

## FINDING CODE SMELLS

![](img/ab52fe24e511e29be73786864ebe0410_95_0.png)

Code that causes a program to crash is obviously wrong, but crashes aren’t the only indicator of issues in your programs. Other signs can suggest the presence of more subtle bugs or unreadable code. Just as the smell of gas can indicate a gas leak or the smell of smoke could indicate a fire, a code smell is a source code pattern that signals potential bugs. A code smell doesn’t necessarily mean a problem exists, but it does mean you should investigate your program.

This chapter lists several common code smells. It takes much less time and effort to prevent a bug than to encounter, understand, and fix a bug later. Every programmer has stories of spending hours debugging only to find that the fix involved changing a single line of code. For this reason, even a whiff of a potential bug should give you pause, prompting you to double-check that you aren't creating future problems.

Of course, a code smell isn't necessarily a problem. Ultimately, whether to address or ignore a code smell is a judgment call for you to make.

### Duplicate Code

The most common code smell is duplicate code. Duplicate code is any source code that you could have created by copying and pasting some other code into your program. For example, this short program contains duplicate code. Notice that it asks how the user is feeling three times:

```
print('Good morning!')
print('How are you feeling?')
feeling = input()
print('I am happy to hear that you are feeling ' + feeling + '.')
print('Good afternoon!')
print('How are you feeling?')
feeling = input()
print('I am happy to hear that you are feeling ' + feeling + '.')
print('Good evening!')
print('How are you feeling?')
feeling = input()
print('I am happy to hear that you are feeling ' + feeling + '.')
```

Duplicate code is a problem because it makes changing the code difficult; a change you make to one copy of the duplicate code must be made to every copy of it in the program. If you forget to make a change somewhere, or if you make different changes to different copies, your program will likely end up with bugs.

The solution to duplicate code is to deduplicate it; that is, make it appear once in your program by placing the code in a function or loop. In the following example, I've moved the duplicate code into a function and then repeatedly called that function:

```
def askFeeling():
    print('How are you feeling?')
    feeling = input()
    print('I am happy to hear that you are feeling ' + feeling + '.')

print('Good morning!')
askFeeling()
print('Good afternoon!')
askFeeling()
print('Good evening!')
askFeeling()
```

In this next example, I've moved the duplicate code into a loop:

```
for timeOfDay in ['morning', 'afternoon', 'evening']:
    print('Good ' + timeOfDay + '!')
    print('How are you feeling?')
    feeling = input()
    print('I am happy to hear that you are feeling ' + feeling + '.')
```

You could also combine these two techniques and use a function and a loop:

```
def askFeeling(timeOfDay):
    print('Good ' + timeOfDay + '!')
    print('How are you feeling?')
    feeling = input()
    print('I am happy to hear that you are feeling ' + feeling + '.')

for timeOfDay in ['morning', 'afternoon', 'evening']:
    askFeeling(timeOfDay)
```

Notice that the code that produces the “Good morning/afternoon/evening!” messages is similar but not identical. In the third improvement to the program, I parameterized the code to deduplicate the identical parts. Meanwhile, the timeOfDay parameter and timeOfDay loop variable replace the parts that differ. Now that I've deduplicated this code by removing the extra copies, I only need to make any necessary changes in one place.

As with all code smells, avoiding duplicate code isn't a hard-and-fast rule you must always follow. In general, the longer the duplicate code section or the more duplicate copies that appear in your program, the stronger the case for deduplicating it. I don't mind copying and pasting code once or even twice. But I generally start to consider deduplicating code when three or four copies exist in my program.

Sometimes, code is just not worth the trouble of deduplicating. Compare the first code example in this section to the most recent one. Although the duplicate code is longer, it's simple and straightforward. The deduplicated example does the same thing but involves a loop, a new timeOfDay loop variable, and a new function with a parameter that is also named timeOfDay.

Duplicate code is a code smell because it makes your code harder to change consistently. If several duplicates are in your program, the solution is to place code inside a function or loop so it appears only once.

### Magic Numbers

It's no surprise that programming involves numbers. But some of the numbers that appear in your source code can confuse other programmers (or you a couple weeks after writing them). For example, consider the number 604800 in the following line:

```
expiration = time.time() + 604800
```

The time.time() function returns an integer representing the current time. We can assume that the expiration variable will represent some point 604,800 seconds into the future. But 604800 is rather mysterious: what's the significance of this expiration date? A comment can help clarify:

```
expiration = time.time() + 604800  # Expire in one week.
```

This is a good solution, but an even better one is to replace these “magic” numbers with constants. Constants are variables whose names are written in uppercase letters to indicate that their values shouldn’t change after their initial assignment. Usually, constants are defined as global variables at the top of the source code file:

```
# Set up constants for different time amounts:
SECONDS_PER_MINUTE = 60
SECONDS_PER_HOUR   = 60 * SECONDS_PER_MINUTE
SECONDS_PER_DAY    = 24 * SECONDS_PER_HOUR
SECONDS_PER_WEEK   = 7  * SECONDS_PER_DAY

--snip--

expiration = time.time() + SECONDS_PER_WEEK  # Expire in one week.
```

You should use separate constants for magic numbers that serve different purposes, even if the magic number is the same. For example, there are 52 cards in a deck of playing cards and 52 weeks in a year. But if you have both amounts in your program, you should do something like the following:

```
NUM_CARDS_IN_DECK = 52
NUM_WEEKS_IN_YEAR = 52

print('This deck contains', NUM_CARDS_IN_DECK, 'cards.')
print('The 2-year contract lasts for', 2 * NUM_WEEKS_IN_YEAR, 'weeks.')
```

When you run this code, the output will look like this:

```
This deck contains 52 cards.
The 2-year contract lasts for 104 weeks.
```

Using separate constants allows you to change them independently in the future. Note that constant variables should never change values while the program is running. But this doesn’t mean that the programmer can never update them in the source code. For example, if a future version of the code includes a joker card, you can change the cards constant without affecting the weeks one:

```
NUM_CARDS_IN_DECK = 53
NUM_WEEKS_IN_YEAR = 52
```

The term magic number can also apply to non-numeric values. For example, you might use string values as constants. Consider the following program, which asks the user to enter a direction and displays a warning if the direction is north. A 'nrth' typo causes a bug that prevents the program from displaying the warning:

```
```python
while True:
    print('Set solar panel direction:')
    direction = input().lower()
    if direction in ('north', 'south', 'east', 'west'):
        break

print('Solar panel heading set to:', direction)
① if direction == 'nrth':
    print('Warning: Facing north is inefficient for this panel.')
```
```

This bug can be hard to detect: the typo in the 'nrth' string ① is still syntactically correct Python. The program doesn't crash, and it's easy to overlook the lack of a warning message. But if we used constants and made this same typo, the typo would cause the program to crash because Python would notice that a NRTH constant doesn't exist:

```
```python
# Set up constants for each cardinal direction:
NORTH = 'north'
SOUTH = 'south'
EAST = 'east'
WEST = 'west'

while True:
    print('Set solar panel direction:')
    direction = input().lower()
    if direction in (NORTH, SOUTH, EAST, WEST):
        break

print('Solar panel heading set to:', direction)
① if direction == NRTH:
    print('Warning: Facing north is inefficient for this panel.')
```
```

The NameError exception raised by the code line with the NRTH typo ① makes the bug immediately obvious when you run this program:

```
```
Set solar panel direction:
west
Solar panel heading set to: west
Traceback (most recent call last):
    File "panelset.py", line 14, in <module>
        if direction == NRTH:
NameError: name 'NRTH' is not defined
```
```

Magic numbers are a code smell because they don't convey their purpose, making your code less readable, harder to update, and prone to undetectable typos. The solution is to use constant variables instead.
