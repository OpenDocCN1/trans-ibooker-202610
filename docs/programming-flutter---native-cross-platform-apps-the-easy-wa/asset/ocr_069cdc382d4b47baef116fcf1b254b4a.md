# Making Your Own Packages

Now that you know how to use a package, you might want to know how to make your own package and how you can write platform-specific code to be used inside a Flutter app.

# CREATING THE PACKAGE

Making a package is a lot like making an app.

If you’re using Android Studio, you just need to use File -> New -> New Flutter Project.

If you only need to use multi-platform Dart code for your package, choose the Flutter Package option, if you also need to add iOS(Objective-C/Swift) or Android(Java/Kotlin) code, choose Flutter Plugin.

Interaction between Dart code and platform-specific code is described in *Integrating Native Code: Making Plugin Packages*.

If you’re using the command line, run this command:

```
flutter create --template=package packagename
```

If you want to create a plugin, replace the word *package* with *plugin*.

# PACKAGE DEVELOPMENT

Creating a normal Dart-only package is as simple as editing and creating Dart files in the lib folder, defining the functions and classes that you want your package to provide, and then publishing it, as explained in *Publishing a Package to Pub*.

# INTEGRATING NATIVE CODE: MAKING PLUGIN PACKAGES

The situation becomes more complicated when you want your package to communicate with platform-specific code, since you’ll need to join together three different codebases written in three different languages.