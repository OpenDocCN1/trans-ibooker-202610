where this alice file is created.

It’s safer to make files in this directory rather that make changes directly to /etc/sudoers; distribution updates could overwrite /etc/sudoers but will never touch files in /etc/sudoers.d. This method also makes it easier to manage and maintain sudo privileges if you make a file for each user getting sudo privileges, as was just done with alice.

+   e. To save and exit, press SPACEBAR, SPACEBAR, and then ENTER. If there is a syntax issue, you’ll see the following menu, which will come after identification of the lines and types of syntax errors (press ENTER at the What Now? prompt):

```
>>> /etc/sudoers.d/alice: syntax error near line 1 <<<
What now?
Options are:
(e)dit sudoers file again
e(x)it without saving changes to sudoers file
(Q)uit and save changes to sudoers file (DANGER!)
```

Q is never an option you should consider. Always choose e or x at this point.

4a–4f

# Step 4 Balancing sudo privileges and security can be done granularly.

Default values of some configuration options can be changed through one or more lines that start with the word Defaults. These values are flags that are turned on by their inclusion and turned off by their inclusion when following a ! symbol.

+   a. Create users named alice, bob, eve, and oscar. Then su to the alice account and try to view the /etc/shadow file. It should work. Try to view the /etc/sudoers file. It shouldn’t work. Try to change the passwords of bob, eve, and oscar. All of those attempts should be successful.

+   b. Go back to the file:

```
sudo visudo -f /etc/sudoers.d/alice
```