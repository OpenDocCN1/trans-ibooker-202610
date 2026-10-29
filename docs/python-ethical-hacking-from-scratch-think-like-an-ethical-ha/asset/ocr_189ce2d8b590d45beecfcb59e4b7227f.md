cd is the name of the command and . . is the path we want to move to. So, on the victim program, we will first use the same check condition to see whether hacker_command starts with cd. If it does, we will strip the command to retrieve the path we want to move into. And finally, we will use the os.chdir() method to navigate to the entered directory if it exists:

```
if hacker_command.startswith("cd"):
    path2move = hacker_command.strip("cd ")
    if os.path.exists(path2move):
        os.chdir(path2move)
    else:
        print("can't change directory to ", path2move)
    continue
```

In Windows, you can see the current directory by giving the pwd (present working directory) command in the shell. Let's now run the hacker and victim programs to see how we can navigate directories:

![](bbox=[0.23, 0.4, 0.77, 0.68])

Figure 6.10 – Changing directory

As you can see in the preceding screenshot, we first navigate up in the folder by using the cd . . command and move to the user folder. Then, we navigate to the Desktop folder by means of the cd Desktop command. This way, we can move up or down in the filesystem. The complete code for the hacker program is given here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py.