To perform a git pull, first navigate to your repository directory within your Command Prompt or terminal, then issue the command git pull. Check out the snippet below from a Bash terminal to see exactly what you need to do, and what output you should expect to see.

```
$ cd somerepository/
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/someuser/somerepository
    86a0b3b..a7cec3d  master     -> origin/master
Updating 86a0b3b..a7cec3d
Fast-forward
    README.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

This example shows that a README.md file has been updated or created from the latest pull.

# Getting an Error?

If you receive fatal: Not a git repository (or any of the parent directories): .git, you’re not in the correct directory. You need cd to your local repository directory – the one in which you cloned your repository to. The majority of Git commands only work when you’re in a Git repository.

# Pull before you Push!

Always git pull on your local copy of your repository before you begin to work. Always! Before you are about to push, do another pull to ensure you have the latest changes.

Remember to talk to your team to coordinate your activity so you are not working on the same files, or use branching⁹⁹ to keep things separate. Branching will then at some point involve merging back to a single branch.