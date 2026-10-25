# This prints

```
Set = {1, 2}
```

This example is a little hard to read, but the following may be clearer. Remember that double open curly braces, {{, and double closed curly braces, }} , cause a literal curly brace to be printed.

# Click here to view code image

```
fss = 'Set = {{ {}, {}, {} }}'
print(fss.format(15, 35, 25))
```

# This prints

```
Set = { 15, 35, 25 }
```

Of course, as long as you have room on a line, you can put everything together:

# Click here to view code image

```
print('Set = {{ {}, {}, {} }}'.format(15, 35, 25))
```

This prints the same output. Remember that each pair of braces defines a print field and therefore causes an argument to be printed, but {{ and }} cause printing of literal braces.

# 5.6 ORDERING BY POSITION