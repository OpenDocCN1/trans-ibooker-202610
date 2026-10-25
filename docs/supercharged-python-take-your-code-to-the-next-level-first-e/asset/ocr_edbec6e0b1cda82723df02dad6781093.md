Notice that a white space after the first name is mandatory,
but the middle initial is followed by a space only if the middle
name is matched. This pattern, if matched against a name, will
optionally recognize a middle initial but not require it. So the
following are all successfully matched:

```
Brian R. Overland
John R. Bennett
John Q. Public
Jane Austen
Mary Shelley
```

In every case, group(name) can be accessed, where name is
'first', 'mid', or 'last'. However, group('mid') in
some cases—where there was no match of that named group—
will return the special value None. But that can be tested for.
Therefore, we can write the following function to break down
a name and reformat it.

Click here to view code image

```
pat = r'(?P<first>\w+) (?P<mid>\w\. )?(?P<last>\w+)'

def reorg_name(in_s):
    m = re.match(pat, in_s)
    s = m.group('last') + ', ' + m.group('first')
    if m.group('mid'):
        s += ' ' + m.group('mid')
    return s
```

By applying this function to each name entered, placing the
result into a list, and then sorting the list, we can store all the