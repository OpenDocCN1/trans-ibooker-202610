# Functional Framework

The two preceding chapters have described the bash idioms in using variables and then combining those variables into expressions. The next level up is grouping those expressions and statements into functions that can be called from various locations within a bash shell script. Even though bash does support the useful construct of functions, it does so in a very bashy way. Let’s see how it differs from what you know from other languages.

# Calling Functions

Here are three statements that are examples of calling bash functions (which we are making up for this example):

```
Do_Something
Find_File 25 $MPATH $ECODE
Show_All $*
```

Those don’t look like function calls, you might be thinking. They look just like any command line invocation of a command.

Exactly.

In other languages, you might say something like Find_File(25, MPATH, ECODE), but that is not bash. In bash, a function is called much like any command; you invoke it like you would invoke a command or a shell script. As with a bash keyword or builtin, the shell doesn’t need to create a separate process to run the function. That makes a function more efficient than calling a separate command binary or shell script.