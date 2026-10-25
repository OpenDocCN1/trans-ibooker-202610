# # 6.10 ITERATIVE SEARCHING ("FINDALL")

One of the most common search tasks is to find _all_ substrings matching a particular pattern. This turns out to be easy, because there is such a function and it produces a Python list.

![](bbox=[0.1445, 0.2766, 0.2539, 0.3547])

Click here to view code image

```
```
list = re.findall(pattern, target_string, flags=0)
```
```

In this syntax, most of the individual items have the meaning described in the previous section: _pattern_ is a regular-expression string or precompiled object, _target_string_ is the string to be searched, and _flags_ is optional.

The return value of `re.findall` is a list of strings, each string containing one of the substrings found. These are returned in the order found.

Regular-expression searches are _non-overlapping_. This means, for example, that once the string “12345” is found, the search will not then find “2345,” “345,” “45,” and so on. Furthermore, all the quantifiers in this chapter are greedy; each will find as long a string as it can.

An example should help clarify. Let’s take an example from the previous section and search for all digit strings, and not only the first one. Also, let’s look for digit strings that have at least