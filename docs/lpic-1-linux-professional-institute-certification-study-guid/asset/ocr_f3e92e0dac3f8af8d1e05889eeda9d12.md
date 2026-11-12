switches are optional. The [FILE] argument shows you can add a directory or filename to the command’s end to look at metadata for either specific files or files within other virtual directory structure locations.

Syntax structure is depicted for many command-line commands within the Linux system’s manual pages, which are also called the man pages. To find a particular command’s syntax structure, view its man page (e.g., man ls) and look in the Synopsis section.

When you issue the ls command with no additional arguments or options, it displays all the files’ and subdirectories’ names within the present working directory as shown in Listing 4.1.

# Listing 4.1: Using the ls and pwd commands

```
$ ls
Desktop  Downloads  Pictures  Public  Videos
Documents Music Project47.txt Templates
$
$ pwd
/home/Christine
$
```

Your present working directory is the current location for your login process within the virtual directory structure. You can determine this location’s directory name by issuing the pwd command, which is also shown in Listing 4.1.

To display more than file and directory name metadata, you need to add various options to the list command. Table 4.1 shows a few of the more commonly used options.

# TABLE 4.1 The ls command’s commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -1 | N/A | List one file or subdirectory name per line |
| -a | --all | Display all file and subdirectory names, including hidden files’ names |
| -d | --directory | Show a directory’s own metadata instead of its contents |
| -F | --classify | Classify each file’s type using an indicator code (*./=>,>@, or |) |
| -i | --inode | Display all file and subdirectory names along with their associated index number |