You might see it in a usage message, telling the user the correct syntax for running the script, or it might be the righthand side of an assignment to a variable. In that later case, we hope that the variable is called something like PROGRAM or SCRIPT because that’s what this expression returns—the name of the script that is executing.

Let’s take a closer look at this particular parameter expansion on $0, one that you can use to get just the basename without all the other parts of the path.

# Path or Prefix Removal

You can remove characters from the front (prefix or lefthand side) or the tail (suffix or righthand side) of that value. To remove a certain set of characters from the left side of a string, you add a # and a shell pattern onto the parameter reference, a pattern that matches those characters that you want to remove.

The expression ${MYVAL#img_} would remove the characters img_ if they were the first characters of the string in the MYVAL variable. Using a more complex pattern, we could write ${MYVAL#*_}. This would remove any sequence of characters up to, and including, an underscore. (If there was no such pattern that matched, its full value is returned unaltered.)

A single # says that it will use the shortest match possible (nongreedy). A double ## says to use the longest match possible (greedy).

Now, perhaps, can you see what the expression ${0#*/} will do?

It will start with the value in $0, the pathname used to invoke the script. Then, from the lefthand side of the value, it will remove the longest match of any number of characters ending in a slash. Thus, it is removing all the parts of the path used in invoking the script, leaving just the name of the script itself.

Here are some possible values for $0 and this pattern we’ve discussed, to see how both the short (#) and long (##) match might differ in results:

| Value in $0 | Expression | Result returned |
|---|---|---|
| ./ascript | ${0#*/} | ascript |
| ./ascript | ${0##*/} | ascript |
| ../bin/ascript | ${0#*/} | bin/ascript |
| ../bin/ascript | ${0##*/} | ascript |
| /home/guy/bin/ascript | ${0#*/} | home/guy/bin/ascript |
| /home/guy/bin/ascript | ${0##*/} | ascript |

Notice that the shortest matching pattern for */ can match just the slash by itself.