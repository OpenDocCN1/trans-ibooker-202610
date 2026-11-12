| Short | Long | Description |
|-------|------|-------------|
| -d | --compare --diff | Compares a tar archive file's members with external files and lists the differences. |
| -t | --list | Displays a tar archive file's contents. |
| -W | --verify | Verifies each file as the file is processed. This option cannot be used with the compression options. |

Verification can take several different forms. You might ensure that the desired files (sometimes called members) are included in the file by using the -v option on the tar command in order to watch the files being listed as they are included in the archive file.
You can also verify that desired files are included in your backup after the fact. Use the -t option to list tarball or archive file contents. An example is shown in Listing 4.34.

# Listing 4.34: Using tar to list a tarball's contents

```
$ tar -tf Project4x.tar.gz
Project42.txt
Project43.txt
Project44.txt
Project45.txt
Project46.txt
$
```

You can verify files within an archive file by comparing them against the current files. The option to accomplish this task is the -d option. An example is shown in Listing 4.35.

# Listing 4.35: Using tar to compare tarball members to external files

```
$ tar -df Project4x.tar.gz
Project42.txt: Mod time differs
Project42.txt: Size differs
$
```

Another good practice is to verify your files automatically immediately after the tar archive is created. This is easily accomplished by tacking on the -W option, as shown in Listing 4.36.