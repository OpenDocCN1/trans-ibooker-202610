case), the function returns a match object; otherwise it returns
the value **None**, which converts to the Boolean value **False**.
You can therefore use the value returned as if it were a
Boolean value. If a match is confirmed, **True** is returned;
otherwise, **False** is returned.

> > Note
> >
> > If you forget to include `r` (the raw-string indicator), this particular example still works,
> > but your code will be more reliable if you always use the `r` when specifying regular-
> > expression patterns. Python string interpretation does not work precisely the way
> > C/C++ string interpretation does. In those languages, every backslash is
> > automatically treated with special meaning unless you use a raw string. (Late
> > versions of C++ also support a raw-string feature.) With Python, certain
> > subexpressions, such as `\n` have special meaning. But otherwise, a backslash is
> > accepted as a literal character.
> >
> > Because Python sometimes interprets a backslash literally and sometimes doesn’t,
> > results can be unreliable and unpredictable, unless you get in the habit of always
> > using raw strings. Therefore, the safe policy is to always place an `r` in front of regular-
> > expression specification strings.

# 6.3 REFINING MATCHES

Although the phone-number example featured in the previous
section works, it has some limitations. The `re.match` function
returns a “true” value any time the pattern matches the
beginning of the target string. It does not have to match the
entire string. So the code confirms a match for the following
phone-number pattern:

```
555-123-5000
```