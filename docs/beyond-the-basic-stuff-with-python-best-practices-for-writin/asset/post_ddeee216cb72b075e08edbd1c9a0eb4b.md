
## Pushing an Existing Repository to GitHub

To push an existing repository from the command line, enter the following:

```
C:\Users\Al\wizcoin>git remote add origin https://github.com/<github_username>/wizcoin.git
C:\Users\Al\wizcoin>git push -u origin master
Username for 'https://github.com': <github_username>
Password for 'https://<github_username>@github.com': <github_password>
Counting objects: 3, done.
Writing objects: 100% (3/3), 213 bytes | 106.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/<your github>/wizcoin.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

The git remote add origin https://github.com/<github_username>/wizcoin.git command adds GitHub as a remote repo corresponding to your local repo. You then push any commits you’ve made on your local repo to the remote repo using the git push -u origin master command. After this first push, you can push all future commits from your local repo by simply running git push. Pushing your commits to GitHub after every commit is a good idea to ensure the remote repo on GitHub is up to date with your local repo, but it’s not strictly necessary.

When you reload the repo’s web page on GitHub, you should see the files and commits displayed on the site. There’s a lot more to learn about GitHub, including how you can accept other people’s contributions to your repos through pull requests. These, along with GitHub’s other advanced features, are beyond the scope of this book.

## Cloning a Repo from an Existing GitHub Repo

It’s also possible to do the opposite: create a new repo on GitHub and clone it to your computer. Create a new repo on the GitHub website, but this time, select the Initialize this repository with a README checkbox.

To clone this repo to your local computer, go to the repo’s page on GitHub and click the Clone or download button to open a window whose URL should look something like https://github.com/<github_username>/wizcoin.git. Use your repo’s URL with the git clone command to download it to your computer:

```
C:\Users\Al>git clone https://github.com/<github_username>/wizcoin.git
Cloning into 'wizcoin'...
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 5 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (5/5), done.
```

You can now commit and push changes using this Git repo just as you would if you had run git init to create the repo.

The git clone command is also useful in case your local repo gets into a state that you don't know how to undo. Although it's less than ideal, you can always save a copy of the files in your working directory, delete the local repo, and use git clone to re-create the repo. This scenario happens so often, even to experienced software developers, that it's the basis of the joke at https://xkcd.com/1597/.

## Summary

Version control systems are lifesavers for programmers. Committing snapshots of your code makes it easy to review your progress and, in certain cases, roll back changes you don't need. Learning the basics of a version control system like Git certainly saves you time in the long run.

Python projects typically have several standard files and folders, and the cookiecutter module helps you create the starting boilerplate for many of these files. These files make up the first files you commit to your local Git repo. We call the folder containing all of this content the *working directory* or *project folder*.

Git tracks the files in your working directory, all of which can exist in one of three states: committed (also called unmodified or clean), modified, or staged. The Git command line tool has several commands, such as git status or git log, that let you view this information, but you can also install several third-party GUI Git tools.

The git init command creates a new, empty repo on your local computer. The git clone command copies a repo from a remote server, such as the popular GitHub website. Either way, once you have a repo, you can use git add and git commit to commit changes to your repo, and use git push to push these commits to a remote GitHub repo. Several commands were also described in this chapter to undo the commits you've made. Performing an undo allows you to roll back to an earlier version of your files.

Git is an extensive tool with many features, and this chapter covers only the basics of the version control system. Many resources are available to you to learn more about Git's advanced features. I recommend two free books that you can find online: *Pro Git* by Scott Chacon at https://git-scm.com/book/en/v2 and *Version Control by Example* by Eric Sink at https://ericsink.com/vcbe/index.html.

## 13

## MEASURING PERFORMANCE AND
BIG O ALGORITHM ANALYSIS

![](img/ab52fe24e511e29be73786864ebe0410_251_0.png)

For most small programs, performance isn’t all that important. We might spend an hour writing a script to automate a task that needs only a few seconds to run. Even if it takes longer, the program will probably finish by the time we’ve returned to our desk with a cup of coffee.

Sometimes it’s prudent to invest time in learning how to make a script faster. But we can’t know if our changes improve a program’s speed until we know how to measure program speed. This is where Python’s timeit and cProfile modules come in. These modules not only measure how fast code runs, but also create a profile of which parts of the code are already fast and which parts we could improve.

In addition to measuring program speed, in this chapter you’ll also learn how to measure the theoretical increases in runtime as the data for your program grows. In computer science, we call this big O notation. Software developers without traditional computer science backgrounds might sometimes feel they have gaps in their knowledge. But although a computer science education is fulfilling, it’s not always directly relevant to software development. I joke (but only half so) that big O notation makes up about 80 percent of the usefulness of my degree. This chapter provides an introduction to this practical topic.

## The timeit Module

“Premature optimization is the root of all evil” is a common saying in software development. (It’s often attributed to computer scientist Donald Knuth, who attributes it to computer scientist Tony Hoare. Tony Hoare, in turn, attributes it to Donald Knuth.) Premature optimization, or optimizing before knowing what needs to be optimized, often manifests itself when programmers try to use clever tricks to save memory or write faster code. For example, one of these tricks is using the XOR algorithm to swap two integer values without using a third, temporary variable:

```
>>> a, b = 42, 101  # Set up the two variables.
>>> print(a, b)
42 101
>>> # A series of ^ XOR operations will end up swapping their values:
>>> a = a ^ b
>>> b = a ^ b
>>> a = a ^ b
>>> print(a, b)  # The values are now swapped.
101 42
```

Unless you’re unfamiliar with the XOR algorithm (which uses the ^ bitwise operator), this code looks cryptic. The problem with using clever programming tricks is that they can produce complicated, unreadable code. Recall that one of the Zen of Python tenets is readability counts.

Even worse, your clever trick might turn out not to be so clever. You can’t just assume a crafty trick is faster or that the old code it’s replacing was even all that slow to begin with. The only way to find out is by measuring and comparing the runtime: the amount of time it takes to run a program or piece of code. Keep in mind that increasing the runtime means the program is slowing down: the program is taking more time to do the same amount of work. (We also sometimes use the term runtime to refer to the period during which the program is running. This error happened at runtime means the error happened while the program was running as opposed to when the program was being compiled into bytecode.)

The Python standard library’s timeit module can measure the runtime speed of a small snippet of code by running it thousands or millions of times, letting you determine an average runtime. The timeit module also temporarily disables the automatic garbage collector to ensure more consistent runtimes. If you want to test multiple lines, you can pass a multiline code string or separate the code lines using semicolons:

```
>>> import timeit
>>> timeit.timeit('a, b = 42, 101; a = a ^ b; b = a ^ b; a = a ^ b')
0.1307766629999998
>>> timeit.timeit('"""a, b = 42, 101
```
