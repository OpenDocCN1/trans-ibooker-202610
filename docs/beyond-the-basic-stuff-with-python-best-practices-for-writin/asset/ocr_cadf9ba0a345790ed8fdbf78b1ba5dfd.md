# The Output

The Tower of Hanoi program displays the towers as ASCII art by using text characters to represent the disks. It might look primitive compared to modern apps, but this approach keeps the implementation simple, because we only need print() and input() calls to interact with the user. When you run the program, the output will look something like the following. The text the player enters is in bold.

```
THE TOWER OF HANOI, by Al Sweigart al@inventwithpython.com

Move the tower of disks, one disk at a time, to another tower. Larger disks cannot rest on top of a smaller disk.

More info at https://en.wikipedia.org/wiki/Tower_of_Hanoi

    ||       ||       ||
    @_1@
    @@_2@@
    @@@_3@@@
    @@@@_4@@@@
    @@@@@_5@@@@@
    A       B       C

Enter the letters of "from" and "to" towers, or QUIT.
(e.g., AB to moves a disk from tower A to tower B.)

> AC
    ||       ||       ||
    @@_2@@
    @@@_3@@@
    @@@@_4@@@@
    @@@@@_5@@@@@
    A       B       @_1@
    C

Enter the letters of "from" and "to" towers, or QUIT.
(e.g., AB to moves a disk from tower A to tower B.)

--snip--

    ||       ||       ||
    ||       ||       @_1@
    ||       ||       @@_2@@
    ||       ||       @@@_3@@@
    ||       ||       @@@@_4@@@@
    ||       ||       @@@@@_5@@@@@
    A       B       C

You have solved the puzzle! Well done!
```

For n disks, it takes a minimum of 2^n − 1 moves to solve the Tower of Hanoi. So this five-disk tower requires 31 steps: AC, AB, CB, AC, BA, BC, AC, AB, CB, CA, BA, CB, AC, AB, CB, AC, BA, BC, AC, BA, CB, CA, BA,