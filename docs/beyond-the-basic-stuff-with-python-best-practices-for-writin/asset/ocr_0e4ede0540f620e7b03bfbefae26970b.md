# Undoing Uncommitted Local Changes

If you’ve made uncommitted changes to a file but want to revert it to the version in the latest commit, you can run git restore <filename>. In the following example, we modify the README.md file but don’t yet stage or commit it:

```
C:\Users\Al\wizcoin>git status
On branch master

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
C:\Users\Al\wizcoin>git restore README.md
C:\Users\Al\wizcoin>git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

After you’ve run the git restore README.md command, the content of README.md reverts to that of the last commit. This is effectively an undo for the changes you’ve made to the file (but haven’t yet staged or committed). But be careful: you can’t undo this “undo” to get those changes back.

You can also run git checkout . to revert all changes you’ve made to every file in your working copy.

# Unstaging a Staged File

If you’ve staged a modified file by running the git add command on it but now want to remove it from staging so it won’t be included in the next commit, run git restore --staged <filename> to unstage it:

```
C:\Users\Al>git restore --staged README.md
Unstaged changes after reset:
M       spam.txt
```

README.md remains modified as it was before git add staged the file, but the file is no longer in the staged state.

# Rolling Back the Most Recent Commits

Suppose you’ve made several unhelpful commits and you want to start over from a previous commit. To undo a specific number of the most recent commits, say, three, use the git revert -n HEAD~3..HEAD command. You can replace the 3 with any number of commits. For example, let’s say you tracked the changes to a mystery novel you were writing and have the following Git log of all your commits and commit messages.