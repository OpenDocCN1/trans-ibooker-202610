In bash, the for loop can be given a list of values to loop over, like this:

```
for num in 1 2 3 4 5; do
    echo "$num"
done
```

Since bash is dealing with strings we don’t have to use just numbers:

```
for person in Sue Neil Pat Harry; do
    echo $person
done
```

Of course the list of values can include variables as well as literals:

```
for person in $ME $3 Pat ${RA[2]} Sue; do
    echo $person
done
```

# QUOTING

You may wonder about quotes in all of these commands. You should, quoting in bash can be quite tricky. Just keep reading, we’ll get to that before the end of this chapter.

Another source of values for the for loop can come from other commands, either a single command or a pipeline of commands:

```
for arg in $( some cmd or other | sort -u)
```

Examples of this kind are:

```
for arg in $(cat /some/file)
for pic in $(find . -name '*.jpg')
for val in $(find . -type d | LC_ALL=C sort)
```

A common use, especially in older scripts, is something like this: