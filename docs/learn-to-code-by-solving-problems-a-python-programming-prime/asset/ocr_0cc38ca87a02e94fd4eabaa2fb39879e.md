```
songs = songs[1:] + songs[0]
elif button == 2:
    songs = songs[-1] + songs[:-1]
elif button == 3:
    songs = songs[1] + songs[0] + songs[2:]

output = ""
for song in songs:
    output = output + song + " "

print(output[:-1])
```

*Listing 4-5: Solving Song Playlist using **break***

The loop’s Boolean expression ❶ looks suspicious: True is always True, so at first glance it seems that this loop never terminates. (That’s the downside to `break`. We can’t just look at the Boolean expression to understand what must happen for the loop to terminate.) But it can terminate, because of our use of break. If button 4 is pressed ❷, then we hit a break ❸, which terminates the loop.

Let’s see one more example of using break. In “while Loops Through Indices” in this chapter, we wrote code to find the index of the first 'y' in a string. Here’s how that looks using break:

```
>>> s = 'zephyr'
>>> i = 0
>>> while i < len(s):
...     if s[i] == 'y':
...         break
...     i = i + 1
...
>>> print(i)
4
```

Again, notice that the loop’s Boolean expression is misleading: it suggests that the loop always runs until the end of the string, but further scrutiny reveals that a break is lurking and can influence termination.

A break terminates only its own loop, not any outer loops. Here’s an example:

```
>>> i = 0
>>> while i < 3:
...     j = 10
...     while j <= 50:
...         print(j)
...         if j == 30:
```