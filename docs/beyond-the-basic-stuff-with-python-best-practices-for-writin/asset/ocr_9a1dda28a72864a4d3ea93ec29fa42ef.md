```
C:\Users\Al\wizcoin>git commit -m "Deleting deleteme.txt from the repo to finish the deletion test."
[master 369de78] Deleting deleteme.txt from the repo to finish the deletion test.
1 file changed, 1 deletion(-)
delete mode 100644 deleteme.txt
C:\Users\Al\Desktop\wizcoin>git status
On branch master
nothing to commit, working tree clean
```

Even though you’ve deleted *deleteme.txt* from your working copy, it still exists in the repo’s history. The “Recovering Old Changes” section later in this chapter describes how to recover a deleted file or undo a change.

The `git rm` command only works on files that are in the clean, committed state, without any modifications. Otherwise, Git asks you to commit the changes or revert them with the `git reset HEAD <filename>` command. (The output of `git status` reminds you of this command ①.) This procedure prevents you from accidentally deleting uncommitted changes.

# Renaming and Moving Files in the Repo

Similar to deleting a file, you shouldn’t rename or move a file in a repo unless you use Git. If you try to do so without using Git, it will think you deleted a file and then created a new file that just happens to have the same content. Instead, use the `git mv` command, followed by `git commit`. Let’s rename the *README.md* file to *README.txt* by running the following commands:

```
C:\Users\Al\wizcoin>git mv README.md README.txt
C:\Users\Al\wizcoin>git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:  README.md -> README.txt

C:\Users\Al\wizcoin>git commit -m "Testing the renaming of files in Git."
[master 3fee6a6] Testing the renaming of files in Git.
1 file changed, 0 insertions(+), 0 deletions(-)
rename README.md => README.txt (100%)
```

This way, the history of changes to *README.txt* also includes the history of *README.md*.

We can also use the `git mv` command to move a file to a new folder. Enter the following commands to create a new folder called *movetest* and move the *README.txt* into it:

```
C:\Users\Al\wizcoin>mkdir movetest
C:\Users\Al\wizcoin>git mv README.txt movetest/README.txt
C:\Users\Al\wizcoin>git status
On branch master
```