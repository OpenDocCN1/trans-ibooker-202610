```
$ git commit -m "Chapter 5 exercises completed"
```

From the example above, you can see that using the -m switch followed by a string provides you with the opportunity to append a message to your commit. Be as explicit as you can, but don’t write too much. People want to see at a glance what you did and do not want to be bored or confused with a long essay. At the same time, don’t be too vague. Simply specifying Updated models.py may tell a developer what file you modified, but they will require further investigation to see exactly what you changed.

# Sensible Commits

Although frequent commits may be a good thing, you will want to ensure that what you have written works before you commit. This may sound silly, but it’s an incredibly easy thing to not think about. To reiterate (for the third time), committing code which doesn’t roll back work can be infuriating to your team members if they then rollback to a version of your project’s codebase which is broken!

# 4. Synchronising your Repository

# Important when Collaborating

Synchronising your local repository before making changes is crucial to ensure you minimise the chance for conflicts occurring. Make sure you get into the habit of doing a pull before you push.

After you’ve committed your local repository and committed your changes, you’re just about ready to send your commit(s) to the remote repository by pushing your changes. However, what if someone within your group pushes their changes before you do? This means your local repository will be out of sync with the remote repository, meaning that any git push command that you issue will fail.

It’s therefore always a good idea to check whether changes have been made on the remote repository before updating it. Running a git pull command will pull down