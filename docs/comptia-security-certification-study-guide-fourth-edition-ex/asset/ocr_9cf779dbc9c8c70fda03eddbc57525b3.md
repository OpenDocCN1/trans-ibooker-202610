# INSIDE THE EXAM

# The Value of Physical Security

For the Security+ exam, know that a hacker can easily bypass the security of a system if they can physically get to a system or server. Any security features of the operating systems are only valid if the operating system is running, so most hackers who wish to bypass the security of the operating system simply boot off a live disc.

In order to prevent this from happening, you need to implement physical security within the organization to ensure that you control who gains physical access to the systems. Remember for the exam that physical security is an important method to help keep unauthorized individuals from gaining access to critical systems and networks.

# EXERCISE 14-1

# Gaining Access to a System with No Physical Security

In this exercise, you will boot off the Windows 10 installation media to create your own administrative account on a Windows 10 system that can then be used to gain access to the system. The point of this exercise is to show you that if someone can get physical access to a system, they can easily bypass most security controls. Ensure you are working on a test Windows 10 system and not on a production system.

+   1. On the Windows 10 VM, boot off the Windows 10 installation media (either ISO file, a DVD, or USB drive).
+   2. Once you’re at the Windows Setup dialog box that asks you the language to install, choose SHIFT-F10 to open a command prompt.
+   3. Type the following commands to locate the partition where Windows was installed. You are looking for the drive that contains the Windows directory (it is most likely the D: drive or E: drive).

```
c:
dir
Is there a Windows directory? ____
d:
dir
Is there a Windows directory? ____
e:
dir
Is there a Windows directory? ____
```