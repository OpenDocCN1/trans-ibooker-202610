(by selecting the icon in the upper-left corner of the Pi OS desktop that looks like two yellow file folders) and create a folder called repository in your Home directory, as shown in the next screenshot.

![](bbox=[0.117, 0.228, 0.883, 0.735])

Next, open a Terminal window and enter the following Git command:

```
$ git init --bare repository/twitchtimer.git
```

If successful, Git will report that it initialized an empty Git repository in the