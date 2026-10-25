negative, rather than positive, way.
There are a number of ways available to get rid of the newline
if it isn't desired. If you've just read all the text from a text file
into a single string, for example, you can remove all the
newlines with the following statement:

```
s = re.sub(r'\n', '', s)
```

If you remove newlines this way and run the example again,
you'll get this output:
Click here to view code image

```
-> See the U.S.A. today.
-> It's right here, not a world away.
-> Average temp. is 70.5.
-> It's fun!
```

# 7.7 NAMED GROUPS

As we explained in Chapter 6, tagged groups are available by
number. The overall string matched is available through the
match object as

```
match_obj.group(0)
```

Individual tagged groups are available by using the numbers
1, 2, 3, and so on. For example, the following refers to the first
tagged group: