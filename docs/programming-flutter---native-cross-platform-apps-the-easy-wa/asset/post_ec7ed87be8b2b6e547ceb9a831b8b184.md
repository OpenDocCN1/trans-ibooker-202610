
replacing /example/path/to/flutter/bin with the path you took note of earlier.

All that’s left to do is to close the file using Ctrl - X and confirming you want to save the file by pressing Y.

To actually be able to run the flutter command, you need to refresh your terminal’s configuration by running:

```
$ source ~/.bash_profile
```

## INSTALLING THE IDE PLUGINS

The Flutter IDE plugins for VSCode and Android Studio are installed using the canonical installation tools and techniques for each IDE. They also require the installation of the respective Dart plugin.

If you’re not familiar with that process, we’ll now discuss how to install the Flutter plugin on each IDE.

### VSCode

To install the Flutter plugin for Visual Studio Code you need to open Visual Studio Code itself, then use the keyboard shortcut Ctrl - Shift - P to open the command palette, then type “Install extensions” and press Enter.

On the panel that opens up on the left, type Flutter and press Enter.

Click the Install button in the Flutter entry in the list, this will install both the Flutter and the Dart plugins.

Unlike the Flutter plugin for Android Studio, the Flutter plugin for Visual Studio Code plugin will auto-detect the location where the Flutter SDK is installed.

### Android Studio

To install the Flutter plugin for Android Studio you need to navigate to File > Settings > Plugins, click Browse repositories..., type Flutter and click Install.

To create a Flutter Project in Android Studio you need to restart Android Studio if you haven’t since installing the Flutter plugin, navigate to File > New Flutter Project,
choose *Flutter Application* and give the app a name and a path, set the package name and you’re done.

## USING THE CLI AND THE PLUGINS

There are two ways of interacting with the Flutter SDK: using the `flutter` CLI command directly or through the IDE plugins. The plugins for Android Studio and VSCode are very different to use, as they adapt to the usual conventions of the respective IDE.

## Create a Flutter App Project

To create a Flutter app project, run the `flutter create appname` command or follow the procedure corresponding to the IDE you want to use to develop your app:

- In Android Studio, click on *File > New > New Flutter Project...* and follow the instructions that will appear on screen;
- In Visual Studio Code, hit `Ctrl + Shift + P` to open the Command Menu and type New Project, *Flutter: New Project* should appear as an option in the menu, click on it and then type a name for your app project.

## Building and Running

You can build a Flutter app using the `flutter build` command when the working directory is the project’s root directory (where `pubspec.yaml` is located):

- To build an APK for your app (standard Android installation file), use `flutter build apk`.
- To build an iOS application bundle, use `flutter build ios`.
- To build an Android App Bundle (the new generation Android installation file that reduces file size), use `flutter build appbundle`.

If you want to run the app directly on an open emulator or a device connected by USB (with USB Debugging turned on in the device’s settings), use `flutter run`.

If you’re using an IDE, you can use the Flutter plugin to run apps directly from the IDE.

To run an app on an open emulator or connected USB device:

- Using VSCode, press `F5` or click on *Debug > Start Debug*.
- Using Android Studio, press Shift F10 and click the button that looks like the one highlighted in the following image:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_27_0.png)

## Hot Reload

Flutter comes with a feature you might know from other frameworks: hot reload. If you’re not familiar with it, the hot reload feature allows you to update the app’s current view based on changes you’ve made to the code without rebuilding the entire app which, as anyone who has ever tried knows, is very painful when you are making many small changes and trying to see how they affect the app.

This feature is accessible in both Android Studio and Visual Studio Code and, actually, also in the command line when you run an app using flutter run: pressing R in the terminal when that command is running will reload the app.

In Android Studio you need to find an icon like this in the Run menu at the bottom:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_27_1.png)

In Visual Studio Code you need to find an icon like this in the menu that appears at the top of the screen when you start debugging:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_27_2.png)

The app will immediately reload, showing any changes you’ve made to the app’s code immediately on the connected emulator or device.

If you’re running a recent version of Android Studio and/or Visual Studio Code, you’ll have IDE access to two different but similar features: stateful hot reload and hot restart. The difference between the two is that the stateful hot reload (as the name implies) preserves each of the State objects and is faster, but it won’t work with bigger changes that require those to be reloaded too, and for that there’s hot restart.

When running an app using flutter run from the CLI, stateful hot reload is associated to the lowercase r character, while hot restart is achieved with the uppercase R character.

In both IDEs you'll find, next to the button shown above, a lightning bolt icon like the one you can see in the following VSCode screenshot:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_28_0.png)

The lightning bolt performs the stateful hot reload, while the typical restart icon performs the hot restart.

## Updating and Maintaining Your Flutter Installation

You'll know when you need to update your Flutter SDK installation because you'll get a notice whenever you build or run an app.

To update your Flutter installation you need to run the flutter upgrade command, which will check and download anything that's needed using Git, so you need to have Git installed.

Flutter includes a tool called Flutter Doctor, available using the flutter doctor command, which will list information about the installed Flutter version, the Android/iOS SDK, the IDE plugins, and connected devices.

## Other Flutter-Related Commands in the VSCode Plugins

Once the VSCode Flutter plugin is installed, opening the command palette and typing flutter will show all available commands.

Among these you'll want to get familiar with:

Flutter: Run Flutter Doctor
Check if there are dependencies that need to be installed.

Flutter: New Project
Creates a new Flutter project.

Flutter: Select Device

Select the device on which you want to debug your app.

> *Flutter: Get Packages and Flutter: Upgrade Packages*

Commands for interacting with Flutter Packages (we’ll discuss packages in Chapter 4).

## Where We’re Going Next: Let’s Start Building Apps

Now that you’re set up with all you need to build Flutter apps, we can start building apps.

You can begin by creating a new Flutter project and familiarizing yourself with the files generated by the SDK as well as the IDE’s UI for managing Flutter projects. Once you feel comfortable working with it, you can move on to the first chapter, in which we’ll learn the structure of a Flutter app and how to implement it.

## Footnotes

[1] https://github.com/google/flutter-desktop-embedding
[2] https://en.wikipedia.org/wiki/Google_Fuchsia
[3] https://flutter.dev/showcase
[4] https://packages.gentoo.org/packages/dev-util/android-studio
[5] https://aur.archlinux.org/packages/android-studio/
[6] https://developer.android.com/studio#downloads
[7] https://aur.archlinux.org/packages/flutter/
[8] https://flutter.dev/docs/development/tools/sdk/archive?tab=linux
[9] https://en.wikipedia.org/wiki/Bash_(Unix_shell)
[10] https://en.wikipedia.org/wiki/Tcsh
[11] https://en.wikipedia.org/wiki/KornShell
[12] https://en.wikipedia.org/wiki/Almquist_shell
[13] https://en.wikipedia.org/wiki/Z_shell
[14] https://developer.android.com/studio#downloads
[15] https://git-scm.com/download/win
[16] https://flutter.dev/docs/development/tools/sdk/archive
[17] https://developer.apple.com/xcode/
[18] https://developer.apple.com/ios/
[19] https://developer.android.com/studio#downloads
[20] https://flutter.dev/docs/development/tools/sdk/archive?tab=macos
