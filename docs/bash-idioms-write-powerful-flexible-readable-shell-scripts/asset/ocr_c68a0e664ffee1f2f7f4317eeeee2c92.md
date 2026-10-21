+   2 We have to declare -A the hash as a bash associative array (again, we say that instead of “hash” here because of the flag).

+   3 Get the count of elements, or the range for the random number we want.

+   4 Use the bash $RANDOM variable to pick a random list element (Example 4-3).

+   5 Echo the random word into the temp file. We used three lines (callouts ③, ④, and ⑤) to do something we could do in one line, like echo "${mylist[$[$RANDOM % ${#mylist[@]}]]}" >> $WORD_FILE, but we think that one line would be a lot harder to reread six months from now.

+   6 Read the file we just created. Note the location of the file name after the done keyword in ⑧.

+   7 Increment the “key” value in hash for the word we saw in the line in the file.

+   8 Note the location of the file name after the done keyword, from the loop in ⑥.

+   9 Iterate over the keys to display a list of the words, without duplicates, and without using the uniq external command. Note the sort command we did use after the done keyword.

+   10 Iterate over the keys again, but this time display the “value” of the count as well.

+   11 Iterate over the keys one last time to display the count again, but this time sort the second field as a number (sort -k2,2n).

Example 7-6 contains the output.

# Example 7-6. bash word count example: output

```
Creating & reading random word list in: /tmp/words.txt

Unique words from: /tmp/words.txt
bar
baz
foo
four
one
three
two

Word counts, ordered by word, from: /tmp/words.txt
bar      7
baz      6
```