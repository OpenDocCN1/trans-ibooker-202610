
> If you are creating directories and moving into them from your present working directory, it is easy to become lost in the directory structure.
> Quickly move back to your previous present working directory using the
> cd - command or back to your home directory using just the cd command
> with no options.

Be aware when building directories that problems can happen when attempting to create
a directory tree, as shown in Listing 4.13.

### Listing 4.13: Avoiding problems with the mkdir command

```
$ ls -F
Everything/ Life/ Project43.txt Project45.txt Universe/
Galaxy/ Project42.txt Project44.txt Project46.txt
$
$ mkdir Projects/42/
mkdir: cannot create directory 'Projects/42/': No such file or directory
$
$ mkdir -p Projects/42/
$
$ ls -F
Everything/ Life/ Project43.txt Project45.txt Projects/
Galaxy/ Project42.txt Project44.txt Project46.txt Universe/
$
$ ls -F Projects
42/
$
```

Notice an error occurs when you attempt to use the mkdir command to build the directory
Projects and its 42 subdirectory. A subdirectory (42) cannot be created without its parent
directory (Projects) preexisting. The mkdir command's -p option (or --parents) allows you
to overwrite this behavior as shown in Listing 4.13 and successfully create directory trees.

> It is tedious to enter the ls -F command after each time you issue the
> mkdir command to ensure that the directory was built. Instead, use the -v
> option on the mkdir command to receive verification that the directory was
> successfully constructed.

## Copying Files and Directories

To copy a file or directory locally, use the cp utility. To issue this command, you use cp
along with any needed options or arguments. The basic syntax structure for the command
is as follows:

```
cp [OPTION]... SOURCE DEST
```

The program’s options, as shown in the structure, are not required. However, the source (SOURCE) and destination (DEST) are required, as shown in a basic cp command example within Listing 4.14.

### Listing 4.14: Using the cp command

```
$ pwd
/home/Christine/SpaceOpera/Emphasis
$ ls
melodrama.txt
$ cp melodrama.txt space-warfare.txt
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

### TABLE 4.2 The cp command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -a | --archive | Perform a recursive copy and keep all the files’ original attributes, such as permissions, ownership, and timestamps. |
| -f | --force | Overwrite any preexisting destination files with same name as DEST. |
| -i | --interactive | Ask before overwriting any preexisting destination files with same name as DEST. |
| -n | --no-clobber | Do not overwrite any preexisting destination files with same name as DEST. |
| -R, -r | --recursive | Copy a directory's contents, and also copy the contents of any subdirectory within the original directory tree (recursive). |
| -u | --update | Only overwrite preexisting destination files with the same name as DEST, if the source file is newer. |
| -v | --verbose | Provide detailed command action information as command executes. |

![](img/ca7617340ddbe2360d36ce3428729fa9_243_0.png)

When you're copying files over a network to a remote host, the file transfer process typically needs protection via encryption methods. OpenSSH can provide the needed data privacy for this type of copy using the scp command (see Chapter 10).

To copy a directory, you need to add the -R (or -r) option to the cp command. This option enacts a recursive copy. A recursive copy will create a new directory (DEST) and copy any files the source directory contains, source directory subdirectories, and their files as well. Listing 4.15 shows an example of how to do a recursive copy as well as how *not* to do one.

### Listing 4.15: Performing a recursive copy with the cp command

```
$ pwd
/home/Christine/SpaceOpera
$
$ ls -F
Emphasis/
$
$ cp Emphasis Story-Line
cp: omitting directory 'Emphasis'
$
$ ls -F
Emphasis/
$
$ cp -R Emphasis Story-Line
$
$ ls -F
Emphasis/ Story-Line/
$
$ ls -R Emphasis
Emphasis:
chivalric-romance.txt    melodrama.txt
interplanetary-battles.txt  space-warfare.txt
$
$ ls -R Story-Line/
Story-Line/:
chivalric-romance.txt    melodrama.txt
interplanetary-battles.txt  space-warfare.txt
$
```

Notice that the first time the cp command is used in Listing 4.15, the -R option is not used, and thus the source directory is not copied. The error message generated, cp: omitting directory, can be a little confusing, but essentially it is telling you that the copy will not take place. When the cp -R command is used to copy the source directory in Listing 4.15, it is successful. The recursive copy option is one of the few command options that can be upper-case, -R, or lowercase, -r.

## Moving/Renaming Files and Directories

To move or rename a file or directory locally, you use a single command: the mv command. The command's basic syntax is nearly the same as the cp command:

```
mv [OPTION]... SOURCE DEST
```

The commonly used mv command options are also similar to cp command options. However, you'll notice in Table 4.3 that there are fewer typical mv command options. As always, be sure to view the mv utility's man pages, using the man mv command, to review all the options for certification studying purposes and explore uncommon options, which may be useful to you.

### TABLE 4.3 The mv command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -f | --force | Overwrite any preexisting destination files with the same name as DEST. |
| -i | --interactive | Ask before overwriting any preexisting destination files with the same name as DEST. |
| -n | --no-clobber | Do not overwrite any preexisting destination files with the same name as DEST. |
| -u | --update | Only overwrite preexisting destination files with the same name as DEST if the source file is newer. |
| -v | --verbose | Provide detailed command action information as the command executes. |

The move command is very simple to use. A few examples of renaming a file as well as employing the -i option to avoid renaming a file to a preexisting file are shown in Listing 4.16.

### Listing 4.16: Using the mv command

```
$ ls
chivalric-romance.txt   melodrama.txt
interplanetary-battles.txt   space-warfare.txt
$
$ mv space-warfare.txt risk-taking.txt
$
$ ls
chivalric-romance.txt   melodrama.txt
interplanetary-battles.txt   risk-taking.txt
$
$ mv -i risk-taking.txt melodrama.txt
mv: overwrite 'melodrama.txt'? n
$
```

When renaming an entire directory, there are no additional required command options. Just issue the mv command as you would for renaming a file, as shown in Listing 4.17.

### Listing 4.17: Renaming a directory using the mv command

```
$ pwd
/home/Christine/SpaceOpera
$
$ ls -F
Emphasis/   Story-Line/
$
$ mv -i Story-Line Story-Topics
$
$ ls -F
Emphasis/   Story-Topics/
$
```

You can move a file and rename it all in one simple mv command as shown in Listing 4.18. The SOURCE uses the file's current directory reference and current name. The DEST uses the file's new location as well as its new name.

### Listing 4.18: Moving and renaming a file using the mv command

```
$ pwd
/home/Christine/SpaceOpera
$ ls
Emphasis  Story-Topics
$ ls Emphasis/
chivalric-romance.txt   melodrama.txt
interplanetary-battles.txt  risk-taking.txt
$
$ ls Story-Topics/
chivalric-romance.txt   melodrama.txt
interplanetary-battles.txt   space-warfare.txt
$
$ mv Emphasis/risk-taking.txt Story-Topics/risks.txt
$
$ ls Emphasis/
chivalric-romance.txt   interplanetary-battles.txt   melodrama.txt
$
$ ls Story-Topics/
chivalric-romance.txt   melodrama.txt   space-warfare.txt
interplanetary-battles.txt   risks.txt
$
```

In Listing 4.18, the file risk-taking.txt is located in the Emphasis directory. Employing a single mv command, it is moved to the Story-Topics directory and renamed to risks.txt at the same time.

## Deleting Files and Directories

Tidying up an entire filesystem or simply your own directory space often starts with deleting unneeded files and directories. Understanding the commands and their switches is paramount to avoid mistakes in removing these items.

The most flexible and heavily used deletion utility is the remove tool. It is employed via the rm command, and the basic syntax is as follows:

```
rm [OPTION]... FILE
```

There are many very useful options for the rm utility, so be sure to view its man pages to see them all. The most commonly used options are listed in Table 4.4.
