

```python
scanner.scan(a_line)
    if unknown:
        print('Unrecognized input:',
              unknown)
        break
    except KeyError as e:
        print('Unrecognized symbol', e.args[0],
              'found in line', pc)
        print(a_list[pc])
        break

main()
```

When this source file is run, it starts the main function, which controls the overall operation of the program. First, it calls the open_rpn_file function, located in the file rpn_io.py.

Because this file is not large and there are relatively few functions, the import * syntax is used here to make all symbolic names in rpn_io.py directly available.

Click here to view code image

```python
#File rpn_io.py
---------------------------------------

def open_rpn_file():
    '''Open-source-file function.
    Open a named file and read lines into a list,
    which is then returned.
    '''
    while True:
        try:
            fname = input('Enter RPN source: ')
            if not fname:
                return None
            f = open(fname, 'r')
            break
        except:
            print('File not found. Re-enter.')
    a_list = f.readlines()
    return a_list

def do_prints(s):
    """Print string function.
    Print string argument s, without adding a
    newline.
    """
    a_str = get_str(s)
    print(a_str, end='')

def do_println(s=''):
    """Print Line function.
    Print an (optional) string and then add a
    newline.
    """
    if s:
        do_prints(s)
    print()

def get_str(s):
    """Get String helper function.
    Get the quoted portion of a string by getting
    text
    from the first quote mark to the last quote
    mark. If
    these aren't present, return an empty string.
    """
    a = s.find('"')
    b = s.rfind('"')
    if a == -1 or b == -1:
        return ''
    return s[a+1:b]

def do_printvar(s, sym_tab):
    """Print Variable function.
    Print named variable after looking it up in
    sym_tab, which was passed from main module.
    '''
    wrd = s.split()[0]
    print(sym_tab[wrd], end=' ')

def do_input(s, sym_tab):
    '''Get Input function.
    Get input from the end user and place it in
    the named variable, using a reference to the
    symbol table (sym_tab) passed in as a reference.
    '''
    wrd = input()
    if '.' in wrd:
        sym_tab[s] = float(wrd)
    else:
        sym_tab[s] = int(wrd)
```

## CHAPTER 14 SUMMARY

In this chapter, we explored various ways of using the `import` statement in Python, to create multiple-module projects that can involve any number of source files.

Using multiple modules in Python does not work in quite the way it works in other languages. In particular, importing in Python is safer if it's *unidirectional*, meaning that `A.py` can import `B.py`, but if so, `B` should not import `A`. You can get away with `A` and `B` importing each other, but only if you know what you're doing and are careful not to create mutual dependencies.

Likewise, you should show some care in importing module-level variables from another module. These are best referred to by their qualified names, as in `mod_a.x` and `mod_a.y`. Otherwise, any assignment to such a variable, outside the module in which it is created, will cause the creation of a new variable that is “local” to the module in which it appears.

Finally, this chapter completed the programming code for the RPN interpreter application that has been developed throughout this book. This chapter added the question mark (?) as a jump-if-not-zero operation, a comparison (>), and the exclamation mark (!) as a random-number generator. Adding these operations greatly expanded the extent of what a script written in RPN can do.

But those additions are far from final. There are many other important features you might want to support, such as line labels and better error checking. These are left as exercises at the end of the chapter.

## CHAPTER 14 REVIEW QUESTIONS

- 1 Is it valid to use more than one `import` statement to import the same module more than once? What would be the purpose? Can you think of a scenario in which it would be useful?

- 2 What are some attributes of a module? (Name at least one.)

- 3 The use of circular importing— such as two modules importing each other— can create dependencies and hidden bugs. How might you design a program to avoid mutual importing?

- 4 What is the purpose of `__all__` in Python?

- 5 In what situation is it useful to refer to the `__name__` attribute or the string `'__main__'`?

- 6 In working with the RPN interpreter application— which interprets an RPN script, line by line— what are some purposes of adding a program counter?

- 7 In designing a simple programming language such as RPN, what are the minimum expressions or statements (or both) that you’d need to make the language primitive but complete— that is, able to make it theoretically possible to carry out any computerized task?

## CHAPTER 14 SUGGESTED PROBLEMS

- 1 Currently, some data is shared between the two modules, `rpn.py` and `rpn_io.py`. Can you revise the application so that common data is placed in a third module, `common.py`?

- 2 Given the way the RPN interpreter program is written, it should be easy to add operations, especially if they correspond to one of the operations defined in the operator package. As a miniproject, add the following: test-for-less-than and test-for-equality. Your biggest challenge may be to find enough punctuation characters to represent all the different operators. However, if you alter the regular expressions used in scanning, you can come up with two-character operators, such as `==` to represent test-for-equality.

- 3 It would be nice for the RPN script writer to be able to add comments to her script. You should be able to implement this feature with the following rule: In each line of an RPN script, ignore all text beginning with a hashtag (#), forward to the end of the line.

- 4 The greater-than test (**>**) is a Boolean operator, producing either True or False (1 or 0). Can the writer of an RPN script produce the same effect as other logical operators without actually being provided with less than (**<**), AND, or OR operators? If you think about it, multiplication (**\*** ) replaces AND beautifully. Does addition (**+**) replace OR as well? For the most part, it does; however, the result is sometimes **2** rather than **1** or **0**. Can you then create a logical NOT operator that takes an input of **0** and produces **1**, but takes any positive number and produces **0**? What we’re really asking here is, Can you think of a couple of arithmetic operations that, when put together, do the same thing as logical OR?

- 5 The biggest piece still missing from the RPN script language is support for line labels. These are not exceptionally difficult to add, but they are not trivial, either. Any line that begins with _label_: should be interpreted as labeling a line of code. To smoothly implement this feature, you should do a couple of passes. The first pass should set up a “code table,” excluding blank lines and compiling a second symbol table that stores labels along with each label’s value; that value should be an index into the code table. For example, 0 would indicate the first line.

- 6 The error checking in this application can be further improved. For example, can you add error checking that reports a syntax error if there are too many operators? (Hint: What would be the state of the stack in that case?)

# 15. Getting Financial Data off the Internet

The last is often best, and we’ve saved the best for last. One of the most impressive things you can do with Python is download financial information and chart it, in a large variety of ways.

This chapter puts together many features used in earlier chapters, showing them off to practical use. You’ll see how to go out and grab information off the Internet, get the desired information on the stock market, and use that data to produce colorful charts showing what your favorite stocks are doing.

It’s going to be a fun ride.

## 15.1 PLAN OF THIS CHAPTER

Three modules make up the stock-market application for this chapter, as shown in Table 15.1. The files are available at **brianoverland.com/books** in downloadable form, along with other files, including the RPN interpreter application.

**Table 15.1.** Modules Used in This Chapter

| Module | Description |
|---|---|
