# TABLE 2.6 The dpkg command actions

| Short | Long | Description |
|---|---|---|
| -c | --contents | Displays the contents of a package file |
| -C | --audit | Searches for broken installed packages and suggests how to fix them |
| N/A | --configure | Reconfigures an installed package |
| N/A | --get-selections | Displays currently installed packages |
| -i | --install | Installs the package; if package is already installed, upgrades it |
| -I | --info | Displays information about an uninstalled package file |
| -l | --list | Lists all installed packages matching a specified pattern |
| -L | --listfiles | Lists the installed files associated with a package |
| -p | --print-avail | Displays information about an installed package |
| -P | --purge | Removes an installed package, including configuration files |
| -r | --remove | Removes an installed package but leaves the configuration files |
| -s | --status | Displays the status of the specified package |
| -S | --search | Locates the package that owns the specified files |

Each action has a set of options that you can use to modify its basic behavior, such as forcing the overwrite of an already installed package or ignoring any dependency errors. To use the dpkg program, you must have the .deb software package available on your system. Often you can find .deb versions of application packages ready for distribution on the application website. Also, most distributions maintain a central location for packages to download.

![](bbox=[0.136, 0.803, 0.222, 0.846])

The Debian distribution also provides a central clearinghouse for Debian packages at www.debian.org/distrib/packages.