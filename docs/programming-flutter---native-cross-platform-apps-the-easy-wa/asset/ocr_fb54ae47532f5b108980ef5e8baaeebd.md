Many native Android/iOS features are available using packages from Pub, but you might need to use a feature that isn’t available in Flutter and that isn’t supplied by any package, so you’ll need to write your own platform-specific code to access that feature.

Although this chapter is about packages, the information in *Connecting Flutter and Platform-Specific Code Together Using Platform Channels* is also useful if you just want to connect Android and iOS code to just one app, without creating a package.

# Creating a Plugin Package

You can create a plugin package using the following command:

```
flutter create --template=plugin packagename
```

This will let you use use Java for Android-specific code and Objective-C for iOS code but, especially in the iOS world, those languages are losing ground to Kotlin (for Android) and Swift (for iOS). Flutter actually also provides a template for those languages too, and if you want to use both Swift and Kotlin, you need to run this command instead:

```
flutter create --template=plugin -a kotlin -i swift packagename
```

As we’ve just seen, you can use flutter’s -a option to specify the language to use for Android code and -i to specify the language for iOS code.

# Connecting Flutter and Platform-Specific Code Together Using Platform Channels

We’ll write the interface for our package in Dart, but we’ll need to connect it to the platform-specific code, and you can do that using platform channels.

Platform channels aren’t exclusive to packages: we can also use them with apps.

The Flutter code calls methods for the platform-specific code to execute through a platform channel, the platform-specific code listens on the channel, receives