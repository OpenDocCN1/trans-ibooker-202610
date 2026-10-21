Many programming languages provide a “switch” or “match” statement, an n-way branch, used as an alternative to a string of if/then/else clauses. There is a similar construct in bash: the case statement. It comes with powerful pattern matching and is very useful in scripting.

# Make Your Case

The keywords case and in delineate the value you want to compare against various patterns. Here’s a simple example:

```
case "$var" in
  yes ) echo "glad you agreed" ;;
  no )
    echo "sorry; good bye"
    exit
  ;;
  * ) echo "invalid answer. try again" ;;
esac
```

...which you can probably figure out. It checks to see if the value in $var is “yes” or “no” and executes the corresponding statement(s). It even has a default action. The end of the case statement is marked by esac, which is case spelled backward. This example is pretty readable, but it just scratches the surface. You’ll also note that you used two different block styles, a “one-liner” for yes and a more typical block (closed by ;;...more on that later) for no. Which you use depends on what you are doing and how the code lines up for readability.