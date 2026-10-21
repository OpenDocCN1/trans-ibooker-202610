# Explicit Values

In bash, the for loop can be given a list of values to loop over, like this:

```
for num in 1 2 3 4 5; do
    echo "$num"
done
```

Since bash is dealing with strings, we aren’t restricted to numbers:

```
for person in Sue Neil Pat Harry; do
    echo $person
done
```

Of course, the list of values can include variables as well as literals:

```
for person in $ME $3 Pat ${RA[2]} Sue; do
    echo $person
done
```

Another source of values for the for loop can come from other commands, either a single command or a pipeline of commands:

```
for arg in $(some cmd or other | sort -u)
```

Examples of this kind are:

```
for arg in $(cat /some/file)
for arg in $(</some/file)    # Faster than shelling out to cat
for pic in $(find . -name '*.jpg')
for val in $(find . -type d | LC_ALL=C sort)
```

A common use, especially in older scripts, is something like this:

```
for i in $(seq 1 10)
```

because the seq command will generate a sequence of numbers. This case could be considered equivalent to:

```
for ((i = 1; i <= 10; i++))
```

This latter for is more efficient and probably more readable. (Note that after the loop terminates, however, the value of i will differ between those two forms [10 versus 11], though generally one doesn’t use the value outside of the loop.)

There’s also this variation, but it has bash version portability issues because the brace expansion was introduced in v3.0 and zero-padding of expanded numeric values was introduced in v4.0:

```
for i in {01..10}; do echo "$i"; done
```