If you are using an older Linux version, you may get a CMake version that is lower than 3.12.1. Then, you need to install it locally. Use the following commands:

```
wget \
https://github.com/Kitware/CMake/releases/download/v3.15.1/cmake-3.15.1-Linux-x86_64.sh

sh cmake-3.15.1-Linux-x86_64.sh
```

When you see the software license, type y and press Enter. When asked about the installation location, type y and press Enter again. This should install it to a new folder in your system.

Now, we will add that folder to our path. Type the following. Note that the first line is a bit too long and the line breaks in this document. You should write it as a single line, as follows:

```
echo "export PATH=\"$HOME/cmake-3.15.1-Linux-x86_64/bin:$PATH\"" >> .bash_profile

source .profile
```

Now, when you type the following:

```
cmake -version
```

You should see the following output:

```
cmake version 3.15.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

3.15.1 is the current latest release at the time of writing this document. Since it is newer than 3.12.1, this will suffice for our purposes.

# Installing Git

Test the current installation by typing the following:

```
git --version
```

You should see a line such as the following:

```
git version 2.17.1
```

If you see the following line instead, you need to install git:

```
command 'git' not found
```