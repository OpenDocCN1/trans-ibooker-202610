```
declare -p myhash
unset -v 'myhash[c]'
declare -p myhash

# Delete the entire hash
unset -v myhash
```

+   1. Declare a hash
+   2. Assign to a hash
+   3. Display or dump the details and values
+   4. Iterate over the keys and values
+   5. Handle slices (sub-sets) of the hash
+   6. Delete elements or the entire hash

That looks like this when you run it:

# Example 4-4. Bash Hash Example: Output

```
The key count is: 5 or 5

The length of the value of key [e] is: 10

Dump or list:
declare -A myhash=([a]="foo" [b]="bar" [c]="baz" [d]="one two" [e]="three four")
${myhash[@] }    = foo|bar|baz|one|two|three|four|
${myhash[*] }    = foo|bar|baz|one|two|three|four|
"${myhash[@]}" = foo|bar|baz|one\ two|three\ four|
"${myhash[*]}" = foo\ bar\ baz\ one\ two\ three\ four|

foreach "${!myhash[@]}":
    Key: a; value: foo
    Key: b; value: bar
    Key: c; value: baz
```