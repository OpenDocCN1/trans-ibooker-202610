Do you think you “get it” now? The first expression tests to see if the length of the value of the variable called DIR is non-zero. If it has a value, that is, if its length is non-zero - and only if it’s non-zero - the cd command will attempt to change into a directory named by the value of DIR.

We could have written this as an explicit IF statement:

```
if [ -n "$DIR" ]; then
  cd "$DIR"
fi
```

To someone less familiar with bash, this latter format is certainly more readable and easily understood. But there isn’t much going on inside the “then” clause, just the cd command, so it seems a bit “bulky” in its syntax. You’ll have to decide which to use based on who your likely readers will be and how likely other commands would be added inside the “then” clause. We’ll offer more opinions on this topic below.

# BASH HELP

The Bash help command is great for any builtin command, but help test provides especially helpful clues about the test commands line -n. You might think to check man bash but if you do you’ll get a very long and somewhat cryptic “all about bash” page. Don’t get us wrong, there is great information in man bash, but help <thing> is probably what you really want. If you are not sure if the command in question is a bash builtin, just try it, or use type -a to find out.

Admittedly, knowing that help test will tell you what -n means is tricky, but then, you were smart enough to buy this book, so now you know. Here is another subtle little tidbit, go try it: help [. You’re welcome.

# Or ELSE...