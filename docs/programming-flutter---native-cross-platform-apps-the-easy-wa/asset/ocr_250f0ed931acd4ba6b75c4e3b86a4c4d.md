If you prefer using the command line or just want a copy-paste experience from this installation guide, you can instead open a terminal window or TTY and run the following command:

```
$ echo "export PATH=$PATH:/home/username/etc/> " >>~/.bash_profile
```

replacing /home/username/etc/ with the path you found earlier for the extracted tarball's flutter/bin directory.

To actually be able to run the flutter command, you need to refresh your terminal's configuration by running:

```
$ source ~/.bash_profile
```

# Installing on Windows

Just like on Linux, Windows-only Android development is supported, so we will install just the Android SDK with Android Studio and the Flutter SDK itself.

# Installing the Android SDK on Windows

All you need to do to install the Android SDK on Windows is to download and run the Android Studio installation file available on the official download page. ${}^{[14]}$ This will also install the feature-rich Android Studio IDE. After installing Android Studio, you will be guided through the installation of the Android SDK.

# Installing the Flutter SDK on Windows

To install the Flutter SDK on Windows you need to install Git for Windows first. You can find the installation file for it on its official download page. ${}^{[15]}$ During installation, you need to choose the Use Git from the Windows Command Prompt option.

After you have installed Git, download the latest version of Flutter from its official installation page. ${}^{[16]}$ This, unlike what happens with most of the software available for Windows, will download a zip archive (a.k.a. a compressed folder) that contains a flutter folder: extract it wherever you want (this will be the SDK folder any IDE plugin will ask you to enter).