
## Writing a Class That implements an Interface

An interface is a class that only defines the return type and arguments of its methods, requiring other classes to implement those methods. Fittingly, this is done using the `implements` keyword:

```
abstract class MyInterface {
    abstract void myMethod();
}
```

```
class MyClass implements MyInterface {
    void myMethod() {
        // do something
    }
}

class MyOtherClass implements MyInterface {
    void myMethod() {
        // do something else
    }
}
```

After doing that, you can instantiate and use the `MyClass` and `MyOtherClass`, and you can use them knowing that they implement all of the methods described in `MyInterface`.

## Typedef and Callbacks

typedef is what is used to create callback types like VoidCallback, which is a void callback type for functions that don’t require arguments. A callback is a kind of closure that is provided as an argument to a function to be called when some conditions are met. A closure is a function that is defined a lot like a variable. The VoidCallback type is a special kind of Function that takes no arguments and has a void return type and it can be defined in the following way using typedef:

```
typedef VoidCallback = void Function();
```

If you wanted to define a callback of a different type that takes some sort of arguments, you’d write the following:

```
typedef IntFunctionCallback = int Function(int n);
```

or the following very common mathematical definition of a real function of a real variable:

```
typedef RealFunctionOfARealVariable = double Function(double n);
```

Definitions can be type-independent (this kind of syntax can be used with anything in Dart):

```
typedef OneArgumentCallback<T> = T Function(T a);
```

After defining such a type or just using the generic Function type, we can create functions just like we create variables. For example, if we wanted to recreate in Dart equivalent the f(x) = e^x exponential function (where e is Euler’s number, better known in some countries as Napier’s number), we would write the following, using pow and e from dart:math:

```
RealFunctionOfARealVariable f = (x) => pow(e, x);

assert(f(2) == pow(e, 2));
assert(f(1) > 2.71 && f(1) < 2.72);
```

## Mastering import

The import directive in Dart, as is the case in many other modern language, doesn’t only handle inclusion of external library files in the code like the good old C #include: it can do more than that to make our life easier when dealing with libraries and packages.

### THE BASICS

We’ll start with the basics: importing an entire file to the current namespace.

### Importing a Local File

You can import another local Dart file by using a relative path to it:

```
import 'filename.dart';
```

### Importing a Built-In Dart Library

You can import a built-in Dart library by prefacing the name of the library with dart::

```
import 'dart:async';
```

### Importing from a Package

You can import a file from a package by using:

```
import 'package:packagename/filename.dart';
```

Which can be used to import files from the Flutter SDK by using flutter as the package name:

```
import 'package:flutter/services.dart'
```

This syntax can also be used to import files in the app’s lib directory by using the app’s package name as the package name.

### Using as to Import Under a Namespace

You can also import under a namespace, by using the following syntax:

```
import 'customlib.dart' as MyCustomLib;
```

So that, for example, if there is a CustomLibraryClass class defined in that file, you can access it by using MyCustomLib.CustomLibraryClass instead of just CustomLibraryClass.

This allows you to import many libraries without cluttering your main namespace, keeping each library separated, without risking having multiple classes with the same name.

Another great advantage comes when you go back to your code after a while: you will know exactly which library that class is from, without having to figure that out first.

### Using show to Only Import Some Parts of a File

import can also be used to only import some classes inside a file.

For example, if you only need the CustomLibraryClass from the customlib.dart file and don’t want to import other classes (maybe because they have the same name as other classes in your namespace and you don’t want to use as and deal with extra dot notation), which you can do with the following code:

```
import 'customlib.dart' show CustomLibraryClass;
```

## Conversion Between Native Java/Apple and Dart Data Types

| Type | Dart | Java | Objective-C/Swift |
|---|---|---|---|
| Integer number | `int` | Integer/Long depending on size | NSNumber NumberWithInt:/NumberWithLong: depending on size |
| Boolean value | `bool` | Boolean | NSNumber NumberWithBool: |
| String of characters | `String` | String | NSString |
| Monodimensional and uniform collection of elements | `List` | ArrayList | NSArray |
| Key-value pair list | `Map` | HashMap | NSDictionary |

## Where You're Going Next

If you have read this appendix before reading the book itself, go back to the beginning and get started! All of the stuff in this appendix will be really easy to apply to building user interfaces using Flutter's great declarative UI syntax.

If you've already read through the rest of the book, the next appendix will show you how to make your apps look native on iOS and how to use the platform-specific configuration to set a custom app name and app icon to prepare your app for distribution.

Footnotes

[55] https://api.dartlang.org/stable/2.3.2/dart-core/List-class.html

## Appendix 2

## Apple-Like Look and Additional App Configuration

There are many things you can’t set in standard Flutter Dart code, like custom app names containing spaces and capitalized letters, as well as custom app icons. There also are many things to be learned about the various features of the pubspec.yaml file.

But first, let’s talk about how you can make your app look more at home on an iPhone by discovering Cupertino widgets.

### Cupertino Widgets

Before we venture out of Flutter Dart code too much, there is one thing you might have noticed in `pubspec.yaml`: the dependency on cupertino_icons. We have actually never used what is contained in it in the body of the book, because it doesn’t really allow you to do anything you couldn’t do before in Flutter apps: it contains icons to be used by widgets that look like native Apple widgets and can be used instead of the Material Design widgets.

This means you have the following two choices regarding the look of your app:

- Make your app look the same on Android and iOS by either using the Material widgets or the Cupertino widgets.
- Make your app look like other apps built for each platform by using Material widgets on Android and Cupertino widgets on iOS.

Obviously you could choose to show your iOS users the Material widgets and the Android users the Cupertino widgets, but that’s probably not the smartest choice for most people.

### THE COUNTER INCREASE APP WITH CUPERTINO WIDGETS

As a way to get an idea of how to use Cupertino widgets, we’re going to rewrite the counter app that Flutter itself gives as an example of basic Flutter UI that we saw in the first chapter, this time using Cupertino widgets instead of Material widgets.

#### What We’re Going to Build

It will look like the following screenshots, keeping in mind that floating action buttons aren’t part of regular Apple Design:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_396_0.png)

![](img/eb93f09b5f47b8f6c9bead1b9047c506_396_1.png)

The alternative would have been to add it to the right of the title in the navigation bar (the Apple app bar), but doing it this way allows us to look at more of the Flutter Cupertino widgets.

### Importing the Cupertino Library

Let’s start by changing the imports, we only need to import the Cupertino Flutter library, given that we won’t be using any Material widgets:

```
cupertino/counter_cupertino/lib/main.dart
```

```
import 'package:flutter/cupertino.dart';
```

### From MaterialApp to CupertinoApp

The MaterialApp is the next thing that needs to change, to become a CupertinoApp. The name isn’t the only thing that needs to change: the theme needs to be of class CupertinoThemeData and not just ThemeData. The CupertinoThemeData takes a primaryColor instead of a primarySwatch, and it needs to one of the CupertinoColors, which aren’t the regular colors you use with Material apps: Colors.blue will become CupertinoColors.activeBlue, which is the default color.

### Material-Based Cupertino Theme Data

Given that you’re probably going to build apps for both Android and iOS using both Material and Cupertino widgets, you might want to create one ThemeData for the Material Design app and derive the styling of the Cupertino version from that Material Design theme. This
