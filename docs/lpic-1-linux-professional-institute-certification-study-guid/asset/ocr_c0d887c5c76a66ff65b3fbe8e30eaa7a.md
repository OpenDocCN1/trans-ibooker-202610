# Using apt-cache

Here are a few useful command options in the apt-cache program for displaying information about packages:

+   - depends: Displays the dependencies required for the package
+   - pkg names: Shows all the packages installed on the system
+   - search: Displays the name of packages matching the specified item
+   - showpkg: Lists information about the specified package
+   - stats: Displays package statistics for the system
+   - unmet: Shows any unmet dependencies for all installed packages or the specified installed package

Typically you can issue the apt-cache commands without employing super user privileges. One handy command is apt-cache pkg names, which displays all installed Debian packages on the system. An example is shown snipped in Listing 2.28. Notice that the pipe symbol (|) and the grep command (both covered in Chapter 1) are employed to quickly determine if any nano packages are currently installed.

# Listing 2.28: Displaying all installed packages with the apt-cache pkg names command

```
$ apt-cache pkg names | grep ^nano
nano
[...]
nano-tiny
[...]
$
```

If you need to look for a particular package to install, the apt-cache search command is useful. A snipped example is shown in Listing 2.29.

# Listing 2.29: Searching for a package with the apt-cache search command

```
$ apt-cache search zsh
zsh - shell with lots of features
zsh-common - architecture independent files for Zsh
zsh-dev - shell with lots of features (development files)
zsh-doc - zsh documentation - info/HTML format
[...]
$
```

When you have found the desired package, peruse its detailed information via the apt-cache showpkg command. The snipped example in Listing 2.30 provides data on the zsh package.