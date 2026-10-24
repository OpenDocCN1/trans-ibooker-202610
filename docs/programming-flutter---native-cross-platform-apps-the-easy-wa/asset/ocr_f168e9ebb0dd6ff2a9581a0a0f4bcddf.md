# Package Usage Example

Now that you know how to install packages, you’ll get to see packages in action.

# GETTING STARTED

We are going to build a simple app that displays the phone’s battery and connectivity state using the battery[28] and connectivity[29] packages from Pub:

![](bbox=[0.15, 0.306, 0.29, 0.505])

To use those packages, we’ll start by adding the packages to our app’s dependencies, and you can do that by adding the following to the dependencies in pubspec.yaml:

```
packages/battery_connectivity/pubspec.yaml

connectivity: ^0.3.1
battery: ^0.2.2
```

And import the packages in our code by adding to the top of the main Dart file the following:

```
packages/battery_connectivity/lib/main.dart

import 'dart:async';

import 'package:flutter/material.dart';
import 'package:battery/battery.dart';
import 'package:connectivity/connectivity.dart';
```

In addition to the packages we download from Pub, we also need Flutter’s standard Material Design API and Dart’s async library, which is needed to properly handle the asynchronous nature of the packages we are using.