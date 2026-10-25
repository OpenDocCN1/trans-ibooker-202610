```
r'[A-Z].*?[.!?](?= [A-Z]|$)'
```

This syntax is getting complex, so let's look at it one piece at a time.

The subexpression `[A-Z]` means that a capital letter must first be read. This will become the first character in the pattern —a sentence—that we’re looking for.

The subexpression `.*?` says, “Match any number of characters.” Because the question mark is added after .*, non-greedy matching is used. This means the sentence will be terminated as soon as possible.

The character set `[.!?]` specifies the end-of-sentence condition. The regular-expression evaluator stops reading a sentence at any one of these marks, subject to the look-ahead condition, considered next. Note that all of these characters lose their special meaning inside square brackets (a character set) and are interpreted as literal characters.

The final part of the pattern is the look-ahead condition: `(?= [A-Z]|$)` . If this condition is not met, the sentence is not complete, and the regular-expression evaluator keeps reading. This expression says, “The _next_ character(s) after this one must consist of a space followed by a capital letter, _or_ by the end of the line or the end of the string. Otherwise, we haven’t reached the end of a sentence.”

> > **Note**
> > As you’ll see in the upcoming examples, looking ahead for an end of line requires the `re.MULTILINE` flag to be correct in all cases.