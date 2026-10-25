# 7.8 THE “RE.SPLIT” FUNCTION

Consider the Reverse Polish Notation (RPN) interpreter introduced in previous chapters. Another way to invoke regular expressions to help analyze text into tokens is to use the `re.split` function.

![](bbox=[0.147, 0.264, 0.255, 0.343])

Click here to view code image

```
list = re.split(pattern, string, maxsplit=0,
flags=0)
```

In this syntax, _pattern_ is a regular-expression pattern supporting all the grammar shown until now; however, it doesn’t specify a pattern to find but to skip over. All the text in between is considered a token. So the _pattern_ is really representative of token separators, and not the tokens themselves.

The _string_, as usual, is the target string to split into tokens. The _maxsplit_ argument specifies the maximum number of tokens to find. If this argument is set to 0, the default, then there is no maximum number.

The action of the `re.split` function is to return a list of strings, in which each string is a token, which in this case is a string of text that appears between occurrences of the indicated search pattern.

It’s common to make the search pattern a space, a series of