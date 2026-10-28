![](bbox=[0.18, 0.09, 0.82, 0.31])

# FIGURE 2-16 Adding a user to groups

Modify a user account by adding it to multiple secondary groups:

```
sudo usermod -a -G pentesters1,pentesters2 jsw
```

This will add jsw to the groups pentesters1 and pentesters2.

Here is the format:

```
usermod -a -G [group1],[group2],[group3] [username]
```

To add a user to secondary groups, use the usermod command with the uppercase -G option.

You can specify many groups at a time separated by commas. Do not include whitespace on either side of the commas, as that would produce an error. Without the -a option, the user’s secondary groups will just be those groups in the command, and any existing secondary group memberships will be removed.

In most instances in any command, you can put multiple options after a single dash and in any order. In this case, -aG would do the same thing, but since the groups follow the -G option, the order must be -aG and not -Ga.

+   c. Find entries in the group file that have the string pentesters:

```
grep pentesters /etc/group
```

grep followed by a string returns only lines from the /etc/group file that contain the string...that follows. The /etc/group file contains a