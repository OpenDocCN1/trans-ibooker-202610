If you choose to use symbolic mode for the chmod command, be aware that it's easy to get the codes confused. For example, don't use the o code thinking that it sets file owner permissions. Instead, it sets other (world) permissions. It would be a bad thing to provide the intended file owner's permissions to the world.

# Using chmod with Octal Mode

The second mode available in chmod is called octal mode. With octal mode the nine permission bits are represented as three octal numbers, one each for the owner, group, and other permissions. Table 4.14 shows how the octal number matches the three symbolic mode permissions.

TABLE 4.14 Octal mode permissions

| Octal value | Permission | Meaning |
|---|---|---|
| 0 | --- | no permissions |
| 1 | --x | execute only |
| 2 | -w- | write only |
| 3 | -wx | write and execute |
| 4 | r-- | read only |
| 5 | r-x | read and execute |
| 6 | rw- | read and write |
| 7 | rwx | read, write, and execute |

You must specify the three octal values in the owner, group, and other in the correct order, as shown in Listing 4.47.

# Listing 4.47: Using octal mode to assign permissions

```
$ chmod 664 research.txt
$
$ ls -l
total 12
-rw-r--r--  1 Christine marketing  1521 Jan 19 15:38 customers.txt
-rw-rw-r--  1 Christine sales      479 Jan 19 15:37 research.txt
-rw-r--r--  1 Christine sales      696 Jan 19 15:37 salesdata.txt
$
```