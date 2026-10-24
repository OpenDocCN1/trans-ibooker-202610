Assets are used in different ways depending on what data you want to load from them.

# LOADING VARIOUS DATA FROM THE ASSETS

To load data from a text-based asset, it needs to be part of an `AssetBundle`, so that you can access the data using the following methods:

+   - `AssetBundle.loadString`, which is used to load strings from text assets.
+   - `AssetBundle.load`, which is used to load bytes from a file.
+   - `AssetBundle.loadStructuredData`, which is used to load any Dart type of data from a file.

All of these return `Futures`, so you’ll need to use the facilities described in Asynchronous Code in Dart: *The dart:async Library* to get the value of the string.

# Where You Can Get an AssetBundle

The basic `AssetBundle` all Flutter apps get is the `rootBundle`, which contains all of the assets we added to pubspec.yaml.

This means you can load a text file off the `rootBundle` directly in the following way:

```
rootBundle.loadString("path/to/file.txt")
```

Alternatively, you can use the `DefaultAssetBundle`, which is actually inherited from the parent `MaterialApp` and can be changed at runtime because it isn’t a `final` variable like the `rootBundle`. The `DefaultAssetBundle` is accessed using the:

```
DefaultAssetBundle.of(context)
```

we used for the `Theme` in the previous chapter. This syntax will be clarified when we discover the full power of the `InheritedWidget` in *InheritedWidgets*.

We can load data from it by using the usual:

```
DefaultAssetBundle.of(context).loadString("path/to/file.txt")
```