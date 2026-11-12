Reading installed packages...

| S | Name | Summary | Type |
|---|---|---|---|
| | nmap | Portscanner | package |
| | nmap4 | A Graphical Front-End for Nmap | package |
| | zenmap | A Graphical Front-End for Nmap | package |

Removing packages with zipper is simple as well. An example of the command, process,
and utility’s helpful messages is shown snipped in Listing 2.20.

# Listing 2.20: Removing a package with the zipper remove command

```
$ sudo zipper remove emacs
[sudo] password for root:
[...]
The following application is going to be REMOVED:
"GNU Emacs"

The following 2 packages are going to be REMOVED:
emacs emacs-x11

2 packages to remove.
After the operation, 99.6 MiB will be freed.
Continue? [y/n/...? shows all options] (y): y
(1/2) Removing emacs-25.3-lp150.2.3.1.x86_64 .....................................[done]
(2/2) Removing emacs-x11-25.3-lp150.2.3.1.x86_64 .............................[done]
There are some running programs that might use files deleted by recent upgrade. You
may wish to check and restart some of them. Run 'zipper ps -s' to list these programs.
$
```

Managing software packages with RPM, YUM, and ZYpp is fairly easy once you
understand when and how to use each utility. The same is true for Debian package
management.

# Using Debian Packages

As you can probably guess, the Debian package management system is mostly used on
Debian-based Linux distros, such as Ubuntu. With this system you can install, modify,
upgrade, and remove software packages. We’ll explore this popular software package man-
agement system in this section.