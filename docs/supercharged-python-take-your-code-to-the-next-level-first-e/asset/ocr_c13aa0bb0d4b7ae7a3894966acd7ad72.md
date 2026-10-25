```
regex1 = re.compile(r'\d{1,3} (?:, \d{3})* (?:\.\d+)?\b')
s = '12,000 monkeys on 100 typewriters for 53.12 days.'
lst = re.findall(regex1, s)
```

# 7.3 GREEDY VERSUS NON-GREEDY MATCHING

One of the subtleties in regular-expression syntax is the issue of greedy versus non-greedy matching. The second technique is also called “lazy.” (Oh what a world, in which everyone is either greedy or lazy!)

The difference is illustrated by a simple example. Suppose we’re searching or matching text in an HTML heading, and the regular-expression evaluator reaches a line of text such as the following:

Click here to view code image

```
the_line = '<h1>This is an HTML heading.</h1>'
```

Suppose, also, that we want to match a string of text enclosed by two angle brackets. Angle brackets are not special characters, so it should be easy to construct a regular-expression search pattern. Here’s our first attempt.

```
pat = r'<.*>'
```