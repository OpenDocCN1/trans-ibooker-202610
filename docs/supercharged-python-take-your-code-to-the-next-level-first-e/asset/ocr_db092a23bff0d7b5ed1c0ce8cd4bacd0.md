The optional **else** clause is executed if the first statement block completes execution with no exceptions. The **finally** clause, if present, is executed after all the other blocks are, unconditionally.

Here’s how you might use these features to open a file for reading, in text mode:

Click here to view code image

```
try:
    fname = input('Enter file to read:')
    f = open(fname, 'r')
    print(f.read())
except FileNotFoundError:
    print('File', fname, 'not found. Terminating.')
```

The use of **except** in this case handles the exception raised if the file can’t be found. By handling this exception, you can terminate nicely or perform other actions. However, it doesn’t automatically reprompt the user for the right name, which is usually what you’d want.

Therefore, you may want to set up a loop that does not terminate until (1) the user enters a file name that is successfully found, or (2) the user indicates he or she wants to quit by entering an empty string.

So, for more flexibility, you can combine **try/except** syntax with a **while** loop. The loop has **break** conditions and so is not truly infinite. It prompts the user until she either enters a valid file name or else she quits by entering an empty string.

Click here to view code image