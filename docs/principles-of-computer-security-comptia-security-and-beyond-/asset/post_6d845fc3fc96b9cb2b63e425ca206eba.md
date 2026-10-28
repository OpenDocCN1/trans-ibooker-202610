
The pair of dots moves you up to the weissman directory, and the /scott moves you down from there to the scott directory. Notice that the jonathan and scott directories are both on the same level, one level below the weissman directory.

j. Move up two directory levels from the current location with a relative reference and the .. characters:

```
cd ../..
```

k. You can include many ../.. combinations with the cd command. Incidentally, entering cd by itself or cd ~ from anywhere moves you back to your home directory.

3a–3f

## Step 3 In this sequence of commands, you’ll copy files in various ways.

Figure 2-3 shows the commands and output.

```
(jonathan@KaliLinuxWeissman)-[~]
$ cd ~/weissman/jonathan

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ touch cscprof

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ ls
cscprof

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ cp cscprof ../scott

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ ls ../scott
cscprof

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ cp cscprof ../scott/cscprof2

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ ls ../scott
cscprof  cscprof2

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ cp ../scott/cscprof ./professor

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ ls
cscprof  professor

(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$
```

FIGURE 2-3 Copying files

- a. Start inside the /home/[your username]/weissman/jonathan directory.
- cd ~/weissman/jonathan
- The ~/ refers to your home directory and isn’t needed if you’re currently there.
- b. Create an empty file:

```
touch cscprof
```

The **touch** command will either change file timestamps (if the file exists) or create an empty file (if the file doesn’t exist). In this case, we’re creating an empty file named cscprof. To create multiple empty files at the same time, you can type **touch** and then type multiple filenames with each separated with whitespace.

c. List the directory contents:

```
ls
```

This verifies that the cscprof file was created (by listing the contents of the current directory).

d. Copy a file with a relative reference:

```
cp cscprof ../scott
ls ../scott
```

This copies the cscprof file to the scott directory (the pair of dots moves you up to weissman, and then you’re able to go down to the scott directory). You can place multiple ../ combinations to go up any number of levels. This technique works for many utilities, including **cp**, **mv**, and **cd**. The first argument, after the **cp** command (which copies both files and directories), is the source file, and the second argument is the target directory. An absolute reference could have been used for the source and destination, but in this case, relative references work better for each. Verify with **ls ../scott**.

e. Copy and rename a file at the same time (from the current directory to another directory):

```
cp cscprof ../scott/cscprof2
ls ../scott
```

Press the UP ARROW key to get the previous command and add **/cscprof2** to the end of that command. This copies the cscprof file to the scott directory (the pair of dots moves you up to weissman; then you’re able to go down to the scott directory) as before, but it also renames the file cscprof to cscprof2 in the process. Verify with **ls ../scott**.

f. Copy and rename a file at the same time (from another directory to the current directory):

```
cp ../scott/cscprof ./professor
ls
```

This copies the cscprof file from the scott directory to this current directory (a single dot represents the current directory) and renames the file cscprof to professor. If you didn’t want to rename the file, a dot would have sufficed for that argument.

Commands, like this **copy** command, can be issued from any directory and don’t require you to be in either the directory of the source or target, as long as you include an absolute or relative reference for the source and target. Verify with **ls**.

4a–4e

## Step 4 In this sequence of commands, you’ll rename and delete files and directories. Figure 2-4 shows the commands and output.

```
(jonathan@KaliLinuxWeissman)-[~/weissman/jonathan]
$ cd

(jonathan@KaliLinuxWeissman)-[~]
$ mv weissman profweissman

(jonathan@KaliLinuxWeissman)-[~]
$ ls
Desktop      Downloads    Pictures     Public       Videos
Documents    Music        profweissman Templates

(jonathan@KaliLinuxWeissman)-[~]
$ touch oldname

(jonathan@KaliLinuxWeissman)-[~]
$ mv oldname newname

(jonathan@KaliLinuxWeissman)-[~]
$ ls
Desktop      Downloads    newname      profweissman Templates
Documents    Music        Pictures     Public       Videos

(jonathan@KaliLinuxWeissman)-[~]
$ rm newname

(jonathan@KaliLinuxWeissman)-[~]
$ ls
Desktop      Downloads    Pictures     Public       Videos
Documents    Music        profweissman Templates

(jonathan@KaliLinuxWeissman)-[~]
$ mkdir hellogoodbye

(jonathan@KaliLinuxWeissman)-[~]
$ rmdir hellogoodbye

(jonathan@KaliLinuxWeissman)-[~]
$ rmdir profweissman/scott
rmdir: failed to remove 'profweissman/scott': Directory not empty

(jonathan@KaliLinuxWeissman)-[~]
$ rm profweissman/scott
rm: cannot remove 'profweissman/scott': Is a directory

(jonathan@KaliLinuxWeissman)-[~]
$ rm -r profweissman/scott

(jonathan@KaliLinuxWeissman)-[~]
$
```

```
(jonathan@KaliLinuxWeissman)-[~]
$ echo Jonathan Scott Weissman
Jonathan Scott Weissman

(jonathan@KaliLinuxWeissman)-[~]
$ echo Jonathan Scott Weissman > rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
Jonathan Scott Weissman

(jonathan@KaliLinuxWeissman)-[~]
$ echo RIT > rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
RIT

(jonathan@KaliLinuxWeissman)-[~]
$ echo FLCC >> rochester

(jonathan@KaliLinuxWeissman)-[~]
$ echo SU >> rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
RIT
FLCC
SU

(jonathan@KaliLinuxWeissman)-[~]
$ tac rochester
SU
FLCC
RIT

(jonathan@KaliLinuxWeissman)-[~]
$ sort rochester
FLCC
RIT
SU

(jonathan@KaliLinuxWeissman)-[~]
$
```

## FIGURE 2-6 A new user

- a. Execute a command as the superuser (**sudo**) and create a new user (**adduser**):

```
sudo adduser jsw
```

The initial user created with a Linux installation is added to the sudo group, which through the /etc/sudoers configuration file (coming up in Lab Exercise 2.04) enables group members to run utilities and programs with root privileges by prefixing the command with the word **sudo**. You’ll see this in action in this step and throughout the rest of the chapter.

Through **sudo** (which originally stood for superuser do), a user can also execute commands as other users (which changed the expansion to substitute user do) besides the superuser, although that’s not as commonly done.

In Linux, superuser refers to the root account, which is locked by default on certain Linux distributions due to the fact that it has no password initially (and it should stay that way, as shown later in this chapter). Therefore, the only way to execute privileged administrative tasks at this point is to run them using **sudo**.

**adduser** is an administrative utility, and that’s why you need to “sudo it.”

At the sudo prompt, enter your password and then select and confirm a password for the new account. Press ENTER for all prompts (including the last question, which defaults to a Y when you press ENTER) from adduser to leave the metadata blank and accept all defaults.

**adduser** is actually a Perl script that uses the native **useradd** binary in the backend, and it is more user friendly and interactive than **useradd**.

On some Linux distributions, however, **adduser** is just a symbolic link (shortcut) to **useradd**.

- b. Change a user’s password:
