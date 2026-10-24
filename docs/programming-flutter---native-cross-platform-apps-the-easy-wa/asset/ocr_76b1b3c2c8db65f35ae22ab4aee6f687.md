and we can check that a string (called `string`) matches that regular expression by using:

```
bool matches = regExp.hasMatch(string)
```

In the case of the regular expression we saw earlier, it will be true if the string contains an operator character, and false if it doesn’t.

# IMPLEMENT ADD()

The first thing we will check is whether the List is empty and only adding the character if it’s not an operator:

```
void add(String added) {
```

```
if(a.isEmpty) {
  if(!regExp.hasMatch(added)) {
    a.add(added);
  }
}
```

```
}
```

If it’s not empty, we need to check whether the previous element was an operator.

In that case, we need to do the same thing we did for the first element—we can’t add another operator or a dot, but we can add a digit and it has to go into a separate List element:

```
else if(regExp.hasMatch(a.last)) {
  if(!RegExp(r"[+\-*/]").hasMatch(added)) {
    a.add(added);
  }
}
```

If the previous conditions haven’t been met (and that means the previous element was a number) we can add anything, but we need to differentiate between operators and digits:

+   - An operator will have to go into its own separate List element;