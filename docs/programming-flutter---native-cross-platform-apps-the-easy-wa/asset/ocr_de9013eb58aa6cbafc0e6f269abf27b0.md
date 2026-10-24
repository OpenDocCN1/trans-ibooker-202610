Now we need to add the executable script to the PATH environment variable.

Before doing this, you need to take note of the directory where you extracted the tarball. It contains a flutter directory, inside which there is a bin directory; we need to know the path to reach that bin directory.

If you are using the GUI, in most distribution it is available by browsing the directory’s properties. It will be something along the lines of /home/username/Downloads/flutter_linux_v1.9.1-stable/flutter/bin if you have gone with the default settings for each piece of software used in the steps we described earlier. I suggest moving this to a more permanent path; ideally one at which you’ll remember you have installed Flutter.

If you worked in the CLI using the commands just outlined, browse to your Flutter installation directory and then change the working directory to the flutter/bin subdirectory by running:

```
$ cd flutter/bin
```

and get the working directory by running:

```
$ pwd
```

which will return something along the lines of /home/username/flutter/bin (here you’ll see the path where you installed Flutter, so your mileage may vary significantly).

# Know Your Shell