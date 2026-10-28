```
pattern = re.compile(r"ecos")
pattern
```

re.compile(r'ecos', re.UNICODE)

We can now use search to locate a substring that matches the pattern.

```
result = pattern.search(string)
print(result)
```

<re.Match object; span=(21, 25), match='ecos'>

The result is either None or an instance of the re.Match class. For convenience, here is a function that displays the key information in a match object:

```
def print_match(result, group_number=0):
    if result is not None:
        # Display the matched substring
        print(f"group({group_number}) = "
              f'{result.group(group_number)}' , end="")
        # Display the starting index and index after end of match
        print(f"start({group_number}) = {result.start(group_number)}  "
              f"end({group_number}) = {result.end(group_number)}")
    else:
        print(None)
```

```
print_match(result)
```

group(0) = ecos"  start(0) = 21   end(0) = 25

I will later describe the role of the group_number argument. For now, note that when group_number is 0, we get information about the entire match.

The function’s comments explain what is displayed, but do note that result.end() shows the index after the matched substring.

# Exercise 12.4

How would you use a slice with result.start() and result.end() to extract the matched substring from the original string?