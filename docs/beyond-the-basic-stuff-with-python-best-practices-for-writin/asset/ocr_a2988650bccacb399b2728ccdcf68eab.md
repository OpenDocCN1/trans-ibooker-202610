This command can display a large amount of text. If the log won’t fit in your Terminal window, it’ll let you scroll up or down using the up and down arrow keys. To quit, press the q key.

If you want to set your files to a commit that’s earlier than the latest one, you need to first find the commit hash, a 40-character string of hexadecimal digits (composed of numbers and the letters A to F), which works as a unique identifier for a commit. For example, the full hash for the most recent commit in our repo is 962a8baa29e452c74d40075d92b00897b02668fb. But it’s common to use only the first seven digits: 962a8ba.

Over time, the log can get very lengthy. The --oneline option trims the output to abbreviated commit hashes and the first line of each commit message. Enter git log --oneline into the command line:

```
C:\Users\Al\wizcoin>git log --oneline
962a8ba (HEAD -> master) Moving the README file back to its original place and name.
3ed22ed Testing the moving of files in Git.
15734e5 Deleting deleteme.txt from the repo to finish the deletion test.
441556a Adding a file to test Git deletion.
2a4c5b8 Added example code to README.md
e1ae3a3 An initial add of the project files.
```

If this log is still too long, you can use -n to limit the output to the most recent commits. Try entering git log --oneline -n 3 to view only the last three commits:

```
C:\Users\Al\wizcoin>git log --oneline -n 3
962a8ba (HEAD -> master) Moving the README file back to its original place and name.
3ed22ed Testing the moving of files in Git.
15734e5 Deleting deleteme.txt from the repo to finish the deletion test.
```

To display the contents of a file as it was at a particular commit, you can run the git show <hash>:<filename> command. But GUI Git tools will provide a more convenient interface for examining the repo log than the command line Git tool provides.

# Recovering Old Changes

Let’s say you want to work with an earlier version of your source code because you’ve introduced a bug, or perhaps you accidentally deleted a file. A version control system lets you undo, or roll back, your working copy to the content of an earlier commit. The exact command you’ll use depends on the state of the files in the working copy.

Keep in mind that version control systems only add information. Even when you delete a file from a repo, Git will remember it so you can restore it later. Rolling back a change actually adds a new change that sets a file’s content to its state in a previous commit. You’ll find detailed information on various kinds of rollbacks at https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/.