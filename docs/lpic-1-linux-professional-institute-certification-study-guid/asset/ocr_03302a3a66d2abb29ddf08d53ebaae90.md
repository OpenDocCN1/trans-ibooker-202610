| Action | Description |
| :--- | :--- |
| dselect-upgrade | Completes any package changes left undone |
| install | Installs or updates a package and updates the package management database |
| remove | Removes a package from the package management database |
| source | Retrieves the source code package for the specified package |
| update | Retrieves updated information about packages in the repository |
| upgrade | Upgrades all installed packages to newest versions |

Installing a new package from the repository is as simple as specifying the package name with the install action. A snipped example of installing the zsh package with apt-get is shown in Listing 2.31.

# Listing 2.31: Installing a package with the apt-get install command

```
$ sudo apt-get install zsh
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  zsh-common
Suggested packages:
  zsh-doc
The following NEW packages will be installed:
  zsh zsh-common
0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
[...]
Unpacking zsh (5.4.2-3ubuntu3.1) ...
Setting up zsh-common (5.4.2-3ubuntu3.1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Setting up zsh (5.4.2-3ubuntu3.1) ...
$
```

If any dependencies are required, the apt-get program retrieves those as well and installs them automatically. Notice in Listing 2.31 that the zsh-common package, which is a zsh dependency, is also installed.