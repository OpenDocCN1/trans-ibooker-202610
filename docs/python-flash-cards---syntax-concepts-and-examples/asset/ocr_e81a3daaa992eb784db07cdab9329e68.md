A package is a directory that contains modules. A package organized to be shared is often called a library. Installing a package gives you access to all its functions and code.

The pip tool installs Python packages and is included in all current Python versions.
To install a package with pip, run pip as a module with the package name:

```
$ python -m pip install --user requests
Collecting requests
--snip--
Installing collected packages: requests
Successfully installed requests-2.18.4
```

The -m flag runs pip as a module. The --user flag installs the package for the current user only. The requests argument is the package name to install.
You’ll see the package download, output of any dependencies needed, confirmation of the installation, and the version.