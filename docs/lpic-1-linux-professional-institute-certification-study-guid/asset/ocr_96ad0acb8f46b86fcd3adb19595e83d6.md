# Listing 4.8: Using a bracketed wildcard with the ls command

```
$ ls
bard bat beat bed bet bEt bird bit bot bunt
$
$ ls b[eio]t
bet bit bot
$
```

Notice that the bracketed wildcard is for the second character position within the filename. Thus, any file that begins with a b and ends with a t is selected, but only if it has an e, i, or o in its second character position. Bracketed wildcards also follow case sensitivity rules when used in this manner. Thus, the file bEt was not selected for display in Listing 4.8.

Just like the question mark, you can use multiple brackets for each character position in the filename you need to wildcard for file globbing. An example of this method is shown in Listing 4.9.

# Listing 4.9: Using multiple bracketed wildcards with the ls command

```
$ ls
bard bat beat bed bet bEt bird bit bot bunt
$
$ ls b[eu][an]t
beat
bunt
$
```

To use bracketed wildcards for a range of characters, instead of typing them all out, you can employ a dash. An example is shown in Listing 4.10.

# Listing 4.10: Using a bracketed range wildcard with the ls command

```
$ ls b[a-z]t
bat bet bEt bit bot
$
```

Be careful here. Notice that using the range nullifies case sensitivity. Thus, in this case, the file bEt was selected for display.

You can negate the character selection by placing a caret symbol (^) in front of your bracketed characters. This causes the command-line program to select any file, except those that have matching characters at the bracketed spot. An example of this is shown in Listing 4.11.

# Listing 4.11: Using a negated bracketed wildcard with the ls command

```
$ ls
bard bat beat bed bet bEt bird bit bot bunt
$
$ ls b[^eio]t
bat bEt
$
```