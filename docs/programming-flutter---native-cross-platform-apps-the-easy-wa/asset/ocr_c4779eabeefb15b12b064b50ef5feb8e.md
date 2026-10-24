# Allowing the User to Click the Image and Go to the Website:  
The url_launcher Package

What if users want to see a comic on the XKCD website? Right now, the only way they could do that is by taking advantage of the comic number and title and searching the XKCD website themselves.

It wouldn’t be too hard to provide users with the ability to do that with a tap in the app, so let’s do it!

To launch URLs from Flutter apps, we’ll use a handy package on Pub called *url_launcher*.

# USING THE URL_LAUNCHER PACKAGE

Let’s add the url_launcher package to the dependencies in pubspec.yaml:

```
```text
networking/xkcd_app/pubspec.yaml
```
```

```
```yaml
url_launcher: ^5.0.2
```
```

and import it in our code in main.dart:

```
```text
networking/xkcd_app/lib/main.dart
```
```

```
```dart
import 'package:url_launcher/url_launcher.dart';
```
```

The *url_launcher* package provides two functions: `canLaunch(url)` and `launch(url)`. The first returns a bool wrapped in a Future, which will be true if there is an app that can handle the URL or false if there isn’t. The second actually launches the URL and returns the same value as the other function: if the launch was successful, it returns true; if it wasn’t, it returns false.

So we know how to launch the URL, but we don’t actually get the comic URL when we fetch the comic’s JSON. That’s not actually a problem, since the URL for each comic is just *https://xkcd.com/comic_number/*: