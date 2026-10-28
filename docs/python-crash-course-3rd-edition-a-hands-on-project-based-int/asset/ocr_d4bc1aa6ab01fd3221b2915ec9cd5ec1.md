We issue the command `git commit -m "message"` ① to make a snapshot of the project. The `-m` flag tells Git to record the message that follows (Started project.) in the project’s log. The output shows that we’re on the `main` branch ② and that two files have changed ③.

When we check the status now, we can see that we’re on the `main` branch, and we have a clean working tree ④. This is the message you should see each time you commit a working state of your project. If you get a different message, read it carefully; it’s likely you forgot to add a file before making a commit.

# Checking the Log

Git keeps a log of all commits made to the project. Let’s check the log:

```
git_practice$ git log
commit ceal3ddc51b885d05a410201a54faf20e0d2e246 (HEAD -> main)
Author: eric <eric@example.com>
Date:   Mon Jun 6 19:37:26 2022 -0800

    Started project.
git_practice$
```

Each time you make a commit, Git generates a unique, 40-character reference ID. It records who made the commit, when it was made, and the message recorded. You won’t always need all of this information, so Git provides an option to print a simpler version of the log entries:

```
git_practice$ git log --pretty=oneline
ceal3ddc51b885d05a410201a54faf20e0d2e246 (HEAD -> main) Started project.
git_practice$
```