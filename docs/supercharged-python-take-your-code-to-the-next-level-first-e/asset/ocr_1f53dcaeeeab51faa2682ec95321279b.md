numbers from o to 9:

Click here to view code image

```
>>> for i in range(10): print(i, end=' ')
0 1 2 3 4 5 6 7 8 9
```

Notice that when you’re within IDLE, this **for** loop is like any
other: You need to type an extra blank line in order to terminate
it.

# ## 4.2.18 Squeeze Multiple Statements onto a Line

If you have a lot of statements you want to squeeze onto the
same line, you can do it—if you’re determined and the
statements are short enough.

The technique is to use a semicolon (;) to separate one
statement on a physical line from another. Here’s an example:

Click here to view code image

```
>>> for i in range(5): n=i*2; m = 5; print(n+m,
end=' ')
5 7 9 11 13
```

You can squeeze other kinds of loops onto a line in this way.
Also, you don’t have to use loops but can place any statements
on a line that you can manage to fit there.

Click here to view code image