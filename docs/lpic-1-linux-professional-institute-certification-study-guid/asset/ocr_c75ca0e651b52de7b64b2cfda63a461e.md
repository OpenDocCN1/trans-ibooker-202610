| Option | Description |
|--------|-------------|
| -k | Set the maximum number of kqueues that can be allocated |
| -l | Set the maximum size of memory that can be locked |
| -m | Set the maximum resident set size |
| -n | Set the maximum number of open file descriptors |
| -p | Set the maximum pipe size in 512k blocks |
| -r | Set the maximum real-time scheduling priority value |
| -s | Set the maximum stack size |
| -t | Set the maximum amount of CPU time the user account is allowed |
| -u | Set the maximum number of processes the user can run simultaneously |
| -v | Set the maximum amount of virtual memory available to the user |
| -x | Set the maximum number of file locks |

As you can tell from Table 10.2, with the ulimit command the Linux administrator can place some pretty severe restrictions on just what an individual user account can do on the system.

The ulimit command is typically placed in the system’s environment files (covered in Chapter 9) so that the limits are set when a user logs into the system.

# # Locating SUID/SGID Files

While the special permissions, SUID and SGID (covered in Chapter 4), are needed by many utilities on a Linux system, it can also be a way for malicious actors to cause problems. Recall that the SUID permission causes the user executing the script to obtain the permissions of the file’s owner. Thus, if the file is owned by the root account, the user essentially becomes root while they are running the script.

It’s a good idea to periodically audit your system to ensure the files that have these two special permissions are supposed to have them. The utility that helps here is the find command (covered in Chapter 4).

The exact command to employ with super user privileges is

```
find / -perm /6000 -type f
```