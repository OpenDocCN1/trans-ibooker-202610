pull command in their respective local workspaces. The git push command isn’t invoked as often as committing – *you require one or more commits to perform a push.* You could aim for one push per day, when a particular feature is completed, or at the request of a team member who is after your updated code.

To push your changes, the simplest command to run is:

```
```bash
$ git push origin master
```
```

As explained in [this Stack Overflow question and answer page](http://stackoverflow.com/questions/7311995/what-is-git-push-origin-master-help-with-gits-refs-heads-and-remotes), this command instructs the git push command to push your local master branch (where your changes are saved) to the *origin* (the remote server from which you originally cloned). If you are using a more complex setup involving [branching and merging](http://git-scm.com/book/en/Git-Branching-Basic-Branching-and-Merging), alter master to the name of the branch you wish to push.

![](bbox=[0.126, 0.431, 0.177, 0.47])

# Important Push?

If your git push is particularly important, you can also alert other team members to the fact they should update their local repositories by pulling your changes. You can do this through a *pull request*. Issue one after pushing your latest changes by invoking the command `git request-pull master`, where master is your branch name (this is the default value). If you are using a service such as GitHub, the web interface allows you to generate requests without the need to enter the command. Check out [the official GitHub website’s tutorial](https://help.github.com/articles/using-pull-requests) for more information.

# Recovering from Mistakes

This section presents a solution to a coder’s worst nightmare: what if you find that your code no longer works? Perhaps a refactoring went wrong, or another team member without discussion changed something. Whatever the reason, using a form of version control always gives you a last resort: rolling back to a previous