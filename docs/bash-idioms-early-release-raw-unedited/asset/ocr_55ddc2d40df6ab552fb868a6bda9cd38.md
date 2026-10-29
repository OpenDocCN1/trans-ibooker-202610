```
$ bash myloop.sh -c 17 core
-c
17
core
$
```

This abbreviated for loop is also very often found in function definitions:

```
function listem {
  for arg; do
    echo "arg to func: '$arg'"
  done
  echo "Inside func: \$0 is still: '$0'"
}
```

Inside a function definition the parameters $1, $2, etc. are the parameters to the function and not parameters to the enclosing shell script. Therefore, inside the function definition the for loop will iterate over the parameters passed to the function.

This minimalist “for” loop iterates over an implied list of values - the parameters passed either to the script or the function. When used in the main body of a script, it iterates over the parameters that were passed to the script; when used inside a shell function, it iterates over the parameters that were passed to that function.

This is definitely one of the obscure bash idioms. You need to know how to read it, but we’ll circle back to debate how to write it a few sections below.

We might like a similarly simple loop but one with explicit values of our own choosing not limited to the parameters. Bash has just the thing.

# Explicit Values