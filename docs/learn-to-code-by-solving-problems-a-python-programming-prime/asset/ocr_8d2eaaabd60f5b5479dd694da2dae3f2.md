You may wonder why I’m maintaining chars_on_line at all. Couldn’t we just use len(line) instead? Well, if we did that, we’d be including spaces in our count, and spaces don’t count toward the number of characters allowed per line. We could fix that by subtracting the count of spaces, and I encourage you to try that on your own if you find it more intuitive than keeping the chars_on_line variable in there.

Now it’s time to loop through all of the words. For each word, we have to determine whether it goes on the current line or next line.

If the number of nonspace characters on the current line plus the number of characters in the current word is at most k, then the current word fits on the current line ⑤. In that case, we add the word plus a space to the current line and update the number of nonspace characters on the line.

Otherwise, the current word doesn’t fit on the current line. The current line is done! We therefore write the line to the output file ⑥ and update the line and chars_on_line variables to reflect that this is the only word on the now-current line.

There are two things to note about the write call ⑥. First, the [:-1] slice is in there to prevent us from outputting the space that follows the last word on the line. Second, you may have expected me to use an f-string here, like this:

```
output_file.write(f'{line[:-1]}\n')
```

However, at the time of writing, the USACO judge is running an older version of Python that doesn’t support f-strings.

Why are we outputting line after the loop ends ⑦? The reason is that each iteration of the for loop is guaranteed to leave line with one or more words that we have not output yet. Consider what happens with each word that we process. If the current word fits on the current line, we don’t output anything. If the current word doesn’t fit on the current line, then we output the current line, but not the word on the next line. We therefore need to write line to the output file after the loop ⑦; otherwise, the last line in the essay will be lost.

The final thing we do is close both files.

One annoying aspect of writing to a file rather than the screen is that we aren’t shown the output when we run the program. To see the output, we have to open the output file in our text editor.