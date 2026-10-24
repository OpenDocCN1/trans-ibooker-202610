If you want to run a command on the Flutter Console, run the `flutter.bat` script contained in the `bin` subfolder.

# **Installing on macOS**

The advantage macOS has is that it also supports iOS building and debugging. To take advantage of that, we also need to install Xcode and the iOS SDK.

# **Installing Xcode and the iOS SDK**

To be able to run Flutter and use it for iOS development you need to download Xcode $^{[19]}$ and the iOS SDK $^{[18]}$ from Apple’s official website.

# **Installing the Android SDK on macOS**

Installing the Android SDK on macOS is really simple: head over to the Android Studio downloads page $^{[19]}$ to download the .dmg file that also includes the feature-rich but resource intensive Android Studio IDE.

# **Installing the Flutter SDK on macOS**

Installing Flutter on macOS is very similar to installing Flutter on Linux. The main differences are that macOS uses the zip archive format instead of the more efficient tar.xz format, and there are no OS or GUI differences among distributions to contend with.

Start by downloading the latest stable .zip from Flutter’s official website. $^{[20]}$

Unzip it to any directory, then open the `flutter` directory you just extracted. Press <kbd>⌘</kbd> + <kbd>↖</kbd> and take note of the path that appears to the right of *Where:*. .

Now, open a Terminal window and run the command:

```
```bash
$ nano ~/.bash_profile
```
```

After doing that, paste the following in the terminal window:

```
```bash
export PATH=$PATH/example/path/to/flutter/bin
```
```