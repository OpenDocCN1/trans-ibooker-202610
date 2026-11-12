# **Listing 2.14:** Reinstalling software with the yum utility

```
# rpm -V emacs
missing    /usr/bin/emacs-24.3
#
# yum reinstall emacs
[...]
---> Package emacs.x86_64 1:24.3-22.el7 will be reinstalled
[...]
Total download size: 2.9 M
Installed size: 14 M
Is this ok [y/d/N]: y

[...]
Installed:
  emacs.x86_64 1:24.3-22.el7

Complete!
#
# rpm -V emacs
#
```

Notice in Listing 2.14 that the `rpm -V emacs` command discovers a missing file in the package. Using the `yum reinstall` feature quickly fixes the issue.
Removing a package with `yum` is just as easy as installing it. An example is shown snipped in Listing 2.15.

# **Listing 2.15:** Removing software with the yum utility

```
# yum remove emacs
[...]
Remove  1 Package

Installed size: 14 M
Is this ok [y/N]: y
[...]
Removed:
  emacs.x86_64 1:24.3-22.el7

Complete!
#
```