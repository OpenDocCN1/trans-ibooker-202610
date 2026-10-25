But notice what happens if we use standard (greedy) matching instead of non-greedy. Remember that greedy matching is enabled by using < .* > instead of < .* ?>.

Click here to view code image

```
pat = r'<.*>'    # Notice use of GREEDY here!
lst = re.findall(pat, s, flags=re.DOTALL)
print('There are', len(lst), 'tags.')
```

This example prints

```
There are 1 tags.
```

That is not the correct answer. The regular expression matched the first opening angle bracket, <, and then kept matching characters until it got to the last and final closing bracket, >, because greedy matching was used.

As a final example in this section, you can use non-greedy matching to help count the number of sentences in a text file. To correctly count sentences, you need to match characters until you get to the nearest period (.) or other end-of-sentence punctuation.

Here’s a sample string that represents the multiline contents of a text file that might have multiple sentences.

Click here to view code image

```
s = '''Here is a single sentence. Here is another sentence, ending in a period. And here is yet another.'''
```