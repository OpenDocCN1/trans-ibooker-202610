# pubspec.yaml

pubspec.yaml is the file that defines all of the information that Dart needs to build the app.

# BASIC PACKAGE DATA

The most basic data contained in pubspec.yaml is the name and the description. The description can contain upper and lower case characters as well as spaces and punctuation, whereas the name must only contain lower case letters and underscores.

Given that the name you set when creating the app project is also the default app name, the user experience isn’t helped by having to select an app that only has lowercase letters and spaces in it, which isn’t really the style that is favored in the mobile industry at the moment (there aren’t many spaces being used, but names are usually capitalized). We’ll be solving this problem at Platform-Specific Setup. In that section we’ll also be seeing how to change the app icon from the default Flutter logo.

# SETTING THE VERSION

One of the things that can be set in pubspec.yaml is the version. If you’re building a package to be released on Pub, it’s obviously going to be used by your users when they are installing the package. In the case of apps, it is automatically added to the platform-specific configuration of each app so that you don’t need to change the app version in multiple files every time.

# Dependency Versions, Caret Syntax, and Semantic Versioning

Versions of Dart dependencies can either be specified by providing just one version number, which will force the installation of that version of the package, as a range using comparison operators (for example, >=1.0.0 < 1.7.3) or by prefixing a version with the ^ character (for example, ^1.2.0), which will allow all versions that are backwards compatible with calls that worked in that version of a library. This is called caret syntax. It was introduced in Dart 1.8.3, which is the reason why the environment section