> **Tip**
> For information on installing PHP in various environments, consult this documentation page: https://www.php.net/manual/en/install.php.

For the purpose of illustration, we choose to discuss PHP 8 installation on two of the main branches of Linux: Debian/Ubuntu and Red Hat/CentOS/Fedora. Let's start with Debian/Ubuntu Linux.

# Installing PHP 8 on Debian/Ubuntu Linux

The best way to install PHP 8 is via the available set of pre-compiled binaries. Newer PHP versions tend to be made available much later than their release date, and PHP 8 is no exception. In this case, it's recommended that you resort to using a (Personal Package Archive(PPA). The PPA hosted at https://launchpad.net/~ondrej is the most extensive and widely used.

If you want to simulate the following steps on your own computer, run an Ubuntu Docker image with PHP 7.4 pre-installed using this command:

```
docker run -it \
  unlikelysource/ubuntu_focal_with_php_7_4:latest /bin/bash
```

In order to install PHP 8 on Debian or Ubuntu Linux, open a command shell onto the production server (or demo container), and, as the root user, proceed as follows. Alternatively, if root user access isn't available, preface each command shown with sudo.

From the command shell, to install PHP 8, proceed as follows:

+   1. Update and upgrade the current set of packages using the apt utility. Any package manager can be used; however, we show the use of apt to maintain consistency between the installation steps covered here:
+   2. Add the Ondrej PPA repository to your apt sources:
+   3. Install PHP 8. This installs only the PHP 8 core and basic extensions:

```
apt update
apt upgrade
```

```
add-apt-repository ppa:ondrej/php
```

```
apt install php8.0
```