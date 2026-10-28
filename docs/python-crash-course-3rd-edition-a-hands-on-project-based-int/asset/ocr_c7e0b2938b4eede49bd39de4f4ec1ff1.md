since the last commit would be restored. This feature is incredibly useful: you can make as many changes as you want when implementing a new feature, and if they don’t work, you can discard them without affecting the project. You don’t have to remember those changes and manually undo them. Git does all of that for you.

> **NOTE**
> 
> *You might have to refresh the file in your editor to see the restored version.*

# # Checking Out Previous Commits

You can revisit any commit in your log, using the checkout command, by using the first six characters of a reference ID. After checking out and reviewing an earlier commit, you can return to the latest commit or abandon your recent work and pick up development from the earlier commit:

```
```text
git_practices$ git log --pretty=oneline
945fa13af128a266d0114eebb7a3276f7d58ecd2 (HEAD -> main) Extended greeting.
cea13ddc51b885d05a410201a54faf20e0d2e246 Started project.
git_practices$ git checkout cea13d
Note: switching to 'cea13d'.
```
```

+   1. You are in 'detached HEAD' state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by switching back to a branch.
+   
+      If you want to create a new branch to retain commits you create, you may do so (now or later) by using `-c` with the switch command. Example: