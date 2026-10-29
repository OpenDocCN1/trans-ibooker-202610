```
[4]="two" [5]="three four")
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two" [4]="three four")
```

Pop LAST element (dumped before and after):

```
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two" [4]="three four")
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two")
```

Delete element 2 using unset (dumped before and after):

```
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two")
declare -a mylist=([0]="bar" [1]="baz" [3]="two")
```

# Hashes

Also known as hashes, dictionaries, or dicts, associative arrays are lists where the index is an arbitrary string instead of an integer. They are amazingly handy for counting or “uniqueing” (that is, ignoring or removing duplicate) strings, among other things.

Unlike lists, these *must* be declared using `declare -A`, `local -A`, or `readonly -A` and the subscript is always required.

*Table 4-2. Sample bash Hash*

| Element | Value |
|---|---|
| myhash[oof] | foo |
| myhash[rab] | bar |
| myhash[zab] | baz |

The common operations on an associative array or hash or dict are:

+   - Declare a variable as an associative array (we say that instead of “hash” here because the flag is -A)
+   - Assign one or more values to it