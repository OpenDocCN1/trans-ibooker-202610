
there’s no indication to Python that the expression is continuing on the next line.)

Python style guides suggest that a line be no longer than 79 characters. A line with the full Boolean expression would squeak in there at 76 characters. But I think the three-line version is clearer, highlighting each condition that must be True on its own line.

We have a good solution here. To explore a little further, let’s discuss some alternate approaches.

Our code uses a Boolean expression to detect when a phone number belongs to a telemarketer. We could have also chosen to write code that detects when a phone number does *not* belong to a telemarketer. If the phone number doesn’t belong to a telemarketer, we should output answer; otherwise, we should output ignore.

If the first digit isn’t 8 and isn’t 9, then the phone number doesn’t belong to a telemarketer. Or, if the fourth digit isn’t 8 and isn’t 9, then the phone number doesn’t belong to a telemarketer. Or, if the second and third digits aren’t equal, then the phone number doesn’t belong to a telemarketer. If even one of these expressions is True, then the phone number doesn’t belong to a telemarketer.

See Listing 2-3 for a version of the code that captures this logic.

```
num1 = int(input())
num2 = int(input())
num3 = int(input())
num4 = int(input())

if ((num1 != 8 and num1 != 9) or
    (num4 != 8 and num4 != 9) or
    (num2 != num3)):
    print('answer')
else:
    print('ignore')
```

*Listing 2-3: Solving Telemarketers, alternate approach*

It’s not easy getting all of those !=, or, and and operators correct! Notice, for example, that we’ve had to change all == operators to !=, all or operators to and, and all and operators to or.

An alternate approach is to use the not operator to negate the “is a telemarketer” expression in one shot. See Listing 2-4 for that code.

```
num1 = int(input())
num2 = int(input())
num3 = int(input())
num4 = int(input())

if not ((num1 == 8 or num1 == 9) and
        (num4 == 8 or num4 == 9) and
        (num2 == num3)):
    print('answer')
else:
    print('ignore')
```

*Listing 2-4: Solving Telemarketers, not operator*

Which of these solutions do you find most intuitive? There’s often more than one way to structure the logic of an if statement, and we should use the one that’s easiest to get right. To me, Listing 2-2 is the most natural, but you may feel otherwise!

Choose your favorite version and submit it to the judge. You should see that all test cases pass.

## Comments

We should always strive to make our programs as clear as possible. This helps to avoid introducing errors when programming and makes it easier to fix our code when errors do slip in. Meaningful variable names, spaces around operators, blank lines to segment the program into its logical pieces, simple if statement logic: all of these practices can improve the quality of the code we write. Another good habit is adding *comments* to our code.

A comment is introduced by the # character and continues until the end of the line. Python ignores comments, so they have no impact on what our program does. We add comments to remind ourselves, or others, about design decisions that we’ve made. Assume that the person reading the code knows Python, so avoid comments that simply restate what the code is doing. Here’s code with an unnecessary comment:

```
>>> x = 5
>>> x = x + 1  # **Increase x by 1**
```

That comment adds nothing beyond what we already know about assignment statements.

See Listing 2-5 for a version of Listing 2-2 with comments.

```
① # ccc18j1, Telemarketers

num1 = int(input())
num2 = int(input())
num3 = int(input())
num4 = int(input())
```

```
② # Telemarketer number: first digit 8 or 9, fourth digit 8 or 9,
# second digit and third digit are same
if ((num1 == 8 or num1 == 9) and
    (num4 == 8 or num4 == 9) and
    (num2 == num3)):
    print('ignore')
else:
    print('answer')
```

Listing 2-5: Solving Telemarketers, comments added

I’ve added three comment lines: the one at the top ① reminds us of the problem code and name, and the two before the if statement ② remind us of the rules for detecting a telemarketer phone number.

Don’t go overboard with comments. Whenever possible, write code that doesn’t require comments in the first place. But for tricky code or to document why you chose to do something in a particular way, a well-placed comment now can save time and frustration later.

## Input and Output Redirection

When you submit Python code to the judge, it runs many test cases to determine whether your code is correct. Is someone there, dutifully waiting for new code and then frantically hammering test cases at it from the keyboard?

No way! It’s all automated. There’s no one typing test cases at the keyboard. How does the judge test our code, then, if we satisfy a call to input by typing something from the keyboard?

The truth is that input isn’t necessarily reading input from the keyboard. It’s reading from a source of input called standard input, which, by default, is the keyboard.

It’s possible to change standard input so that it refers to a file rather than the keyboard. The technique is called input redirection, and it’s what the judge uses to provide input.

We can also try input redirection ourselves. For programs whose input is small—just a line of text or a couple of integers—input redirection may not save us much. But for programs whose test cases can be tens or hundreds of lines long, input redirection makes it much easier to test our work. Rather than typing the same test case over and over, we can store it in a file and then run our program on it as many times as we want.

Let’s try input redirection on Telemarketers. Navigate to your *programming* folder and create a new file called *telemarketers_input.txt*. In that file, type the following:

```
8
1
1
9
```

The problem specifies that we should provide one integer per line, so we’ve written them one per line here.

Save the file. Now enter **python telemarketers.py < telemarketers_input.txt** to run your program using input redirection. Your program should output ignore, just as it would if you’d typed the test case from the keyboard.

The < symbol instructs your operating system to use a file rather than the keyboard to provide input. After the < symbol comes the name of the file that contains the input.

To try your program on different test cases, just modify the *telemarketers_input.txt* file and run your program again.

We can also change where our output goes, though we won’t need to for this book. The print function outputs to *standard output*, which, by default, is the screen. We can change standard output so that it instead refers to a file. We do so using *output redirection*, which is written as a > symbol followed by a filename.

Enter **python telemarketers.py > telemarketers_output.txt** to run your program using output redirection. Provide four integers of input, and you should be back to your operating system prompt. But you shouldn’t see any output from the Telemarketers program! That’s because we’ve redirected the output to file telemarketers_output.txt. If you open telemarketers_output.txt in your text editor, you should see the output there.

Be careful with output redirection. If you use a filename that already exists, your old file will be overwritten! Always double-check that you’re using the filename you intended.

## Summary

In this chapter, we learned how to use if statements to direct what our programs do. The key ingredient of an if statement is a Boolean expression, which is an expression with a True or False value. To build up Boolean expressions, we use relational operators such as == and >=, and we use Boolean operators such as and and or.

Deciding what to do based on what is True and False makes our programs more flexible, able to adapt to the situation at hand. But our programs are still limited to handling small amounts of input and output—whatever we can do with individual calls to input and print. In the next chapter, we’ll start learning about loops, which let us repeat code so that we can process as much input and output as we like.

Want to work with 100 values? How about 1,000? And with just a small amount of Python code? It is a little early for me to be provoking you, I know, because you still have the following exercises to do. But when you’re ready, read on!

## Chapter Exercises

Here are some exercises for you to try.

- 1. DMOJ problem ccc06j1, Canadian Calorie Counting
- 2. DMOJ problem ccc15j1, Special Day
- 3. DMOJ problem ccc15j2, Happy or Sad
- 4. DMOJ problem dmopc16c1p0, C.C. and Cheese-kun
- 5. DMOJ problem ccc07j1, Who is in the Middle

## Notes

Winning Team is originally from the 2019 Canadian Computing Competition, Junior Level. Telemarketers is originally from the 2018 Canadian Computing Competition, Junior Level.
