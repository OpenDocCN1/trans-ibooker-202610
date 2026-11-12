The program’s options, as shown in the structure, are not required. However, the source (SOURCE) and destination (DEST) are required, as shown in a basic cp command example within Listing 4.14.

# Listing 4.14: Using the cp command

```
$ pwd
/home/Christine/SpaceOpera/Emphasis
$ $ ls
melodrama.txt
$ $ cp melodrama.txt space-warfare.txt
$
$ ls
melodrama.txt space-warfare.txt
$
$ cp melodrama.txt
cp: missing destination file operand after 'melodrama.txt'
Try 'cp --help' for more information.
$
```

In Listing 4.14, the first time the cp command is used, both the source file and its destination are specified. Thus, no problems occur. However, the second time the cp command is used, the destination filename is missing. This causes the source file to not be copied and generates an error message.

There are several useful cp command options. Many will help protect you from making a grievous mistake, such as accidentally overwriting a file or its permissions. Table 4.2 shows a few of the more commonly used options.

# TABLE 4.2 The cp command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -a | --archive | Perform a recursive copy and keep all the files’ original attributes, such as permissions, ownership, and timestamps. |
| -f | --force | Overwrite any preexisting destination files with same name as DEST. |
| -i | --interactive | Ask before overwriting any preexisting destination files with same name as DEST. |
| -n | --no-clobber | Do not overwrite any preexisting destination files with same name as DEST. |