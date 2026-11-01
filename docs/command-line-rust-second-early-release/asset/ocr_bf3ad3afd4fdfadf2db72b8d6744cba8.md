To start verify for yourself that `grep fox empty.txt` will print nothing when using the *empty.txt* file. As shown by the usage, `grep` accepts a regular expression as the first positional argument and possibly some input files for the rest. Note that an empty regular expression will match all lines of input, and here I’ll use the input file *fox.txt*, which contains one line of text:

```
```
$ grep "" fox.txt
The quick brown fox jumps over the lazy dog.
```
```

Take a peek at this lovely Emily Dickinson poem, and notice that “Nobody” is always capitalized:

```
```
$ cat nobody.txt
I'm Nobody! Who are you?
Are you–Nobody–too?
Then there's a pair of us!
Don't tell! they'd advertise–you know!

How dreary–to be–Somebody!
How public–like a Frog–
To tell one's name–the livelong June–
To an admiring Bog!
```
```

If I search for *Nobody*, the two lines containing the string are printed:

```
```
$ grep Nobody nobody.txt
I'm Nobody! Who are you?
Are you–Nobody–too?
```
```

If I search for lowercase “nobody” with `grep nobody nobody.txt`, nothing is printed. I can, however, use `-i|--ignore-case` to find these lines:

```
```
$ grep -i nobody nobody.txt
I'm Nobody! Who are you?
Are you–Nobody–too?
```
```