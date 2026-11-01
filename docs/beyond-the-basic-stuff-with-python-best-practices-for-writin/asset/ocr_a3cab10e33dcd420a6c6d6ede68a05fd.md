Other than this one line change to main(), the rest of the program works the same as before. When you run the program now, the output will look something like this:

```
```
Welcome to Tic-Tac-Toe!
Use mini board? Y/N: y

    ... 123
    ... 456
    ... 789
What is X's move? (1-9)
1

    X.. 123
    ... 456
    ... 789
What is O's move? (1-9)
--snip--
    XXX 123
    .OO 456
    O.X 789
X has won the game!
Thanks for playing!
```
```

Your program can now easily have both implementations of these tic-tac-toe board classes. Of course, if you only want the mini version of the board, you could simply replace the code in the getBoardStr() method for TTTBoard. But if you need both, inheritance lets you easily create two classes by reusing their common code.

If we didn't use inheritance, we could have, say, added a new attribute to TTTBoard called useMiniBoard and put an if-else statement inside getBoardStr() to decide when to show the regular board or the mini one. This would work well for such a simple change. But what if the MiniBoard subclass needed to override 2, 3, or even 100 methods? What if we wanted to create several different subclasses of TTTBoard? Not using inheritance would cause an explosion of if-else statements inside our methods and a large increase in our code's complexity. By using subclasses and overriding methods, we can better organize our code into separate classes to handle these different use cases.

# The super() Function

A child class's overridden method is often similar to the parent class's method. Even though inheritance is a code reuse technique, overriding a method might cause you to rewrite the same code from the parent class's method as part of the child class's method. To prevent this duplicate code, the built-in super() function allows an overriding method to call the original method in the parent class.

For example, let's create a new class called HintBoard that subclasses TTTBoard. The new class overrides getBoardStr(), so after drawing the tic-tac-toe board, it also adds a hint if either X or O could win on their next move. This means that the HintBoard class's getBoardStr() method has to do