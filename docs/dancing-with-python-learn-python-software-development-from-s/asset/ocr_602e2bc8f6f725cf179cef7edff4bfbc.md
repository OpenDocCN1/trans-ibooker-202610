If you are only interested in knowing whether one **str** is contained within another, use **in**.

```
"dream" in quote
```

True

```
"nightmare" in quote
```

False

To test whether a string is not contained in another, use **not in**.

```
"Hamlet" not in quote
```

True

# 2.7.3 Slicing and extracting substrings

We’ve just seen how to access individual characters and find the position of substrings in a **str**. We use *slice notation* to extract portions of strings and return them as new strings.

For our quote

```
quote
```

'perchance to dream'

we extract the substring starting at index 3 and of length 6 by executing

```
quote[3:9]
```

'chance'

The number before the colon is the starting index, and the number after the colon is the starting index plus the substring length. The expression **quote[3:9]** means

> Beginning at index 3, collect together all characters in
> quote at that index and the valid indices less than 9.

![](bbox=[0.133, 0.787, 0.867, 0.872])