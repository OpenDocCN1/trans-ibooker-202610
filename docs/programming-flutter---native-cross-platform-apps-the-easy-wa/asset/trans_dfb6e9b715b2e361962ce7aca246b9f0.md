

```
color: _batteryColor,
fontSize: 40.0,
),
),
```

结果是每种电池状态都由不同的图标和不同颜色的字符串表示：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_176_0.png)

以及每种连接状态：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_176_1.png)

## 制作你自己的包

既然你已经知道如何使用包，你可能想知道如何制作自己的包，以及如何编写可在 Flutter 应用内使用的平台特定代码。

### 创建包

制作一个包与制作一个应用非常相似。

如果你使用的是 Android Studio，只需使用“文件 -> 新建 -> 新建 Flutter 项目”。

如果你的包只需要使用多平台 Dart 代码，请选择 Flutter Package 选项；如果你还需要添加 iOS（Objective-C/Swift）或 Android（Java/Kotlin）代码，请选择 Flutter Plugin。

Dart 代码与平台特定代码之间的交互在*集成原生代码：制作插件包*中有描述。

如果你使用命令行，请运行以下命令：

```
flutter create --template=package packagename
```

如果你想创建一个插件，请将*包*一词替换为*插件*。

### 包开发

创建一个普通的纯 Dart 包非常简单，只需在 lib 文件夹中编辑和创建 Dart 文件，定义你希望包提供的函数和类，然后发布它，如*将包发布到 Pub*中所解释的那样。

### 集成原生代码：制作插件包

当你希望你的包与平台特定代码通信时，情况会变得更加复杂，因为你需要将三个用不同语言编写的不同代码库连接在一起。

许多 Android/iOS 原生功能都可以通过使用 Pub 上的包来实现，但你可能需要一个 Flutter 中不可用、也没有任何包提供的功能，因此你需要编写自己的平台特定代码来访问该功能。

虽然本章是关于包的，但如果你只想将 Android 和 iOS 代码连接到一个应用而不创建包，那么*使用平台通道将 Flutter 和平台特定代码连接在一起*中的信息也很有用。

### 创建插件包

你可以使用以下命令创建插件包：

```
flutter create --template=plugin packagename
```

这将允许你使用 Java 编写 Android 特定代码，使用 Objective-C 编写 iOS 代码，但特别是在 iOS 领域，这些语言正逐渐被 Kotlin（用于 Android）和 Swift（用于 iOS）取代。Flutter 实际上也为 Kotlin 和 Swift 提供了模板，如果你希望同时使用 Swift 和 Kotlin，需要改为运行以下命令：

```
flutter create --template=plugin -a kotlin -i swift packagename
```

正如我们刚才看到的，你可以使用 Flutter 的 -a 选项来指定用于 Android 代码的语言，使用 -i 选项来指定用于 iOS 代码的语言。

### 使用平台通道将 Flutter 和平台特定代码连接在一起

我们将用 Dart 为包编写接口，但需要将它连接到平台特定代码，你可以使用平台通道来实现。

平台通道并非包独有：我们也可以在应用中使用它们。

Flutter 代码通过平台通道调用平台特定代码要执行的方法，平台特定代码在该通道上监听，接收方法调用，执行所需操作，并返回响应。

### 平台通道的工作原理

为了连接到我们的平台特定代码，我们将使用 MethodChannel 类，它是 Flutter 与平台特定代码之间接口中 Flutter 的一侧。

我们向其构造函数传递通道名称，这是该通道在整个应用中的唯一字符串标识符。

由于它必须在整个应用中唯一，通道名称通常以域名作为前缀。

该通道的工作方式如下：Dart 代码在通道上调用方法，平台特定代码在通道上监听，等待方法调用，方法调用由方法调用处理程序处理，处理程序将结果发送回 Dart 代码。

Dart 代码以 Future 的形式接收响应。

### 在 Dart 中实现 Flutter 接口

以下是一个可以与平台特定代码交互的基本 Flutter 接口示例：

`packages/making_plugins/lib/plugin_example.dart`

```dart
import 'dart:async';
import 'package:flutter/services.dart';

class PluginExample {
  static const _platformChannel =
    MethodChannel('example.com/plugin_example');
  static Future<type> exampleMethod() async {
    return await _platformChannel.invokeMethod('runExampleMethod');
  }
}
```

我们可以使用 services 库访问平台通道，还需要使用 async 库来处理 Future，因此需要将以下行放在 Dart 文件顶部：

`packages/making_plugins/lib/plugin_example.dart`

```dart
import 'dart:async';
import 'package:flutter/services.dart';
```

然后我们定义包含代码的类，并首先定义我们将要使用的通道，即 example.com/plugin_example：

`packages/making_plugins/lib/plugin_example.dart`

```dart
static const _platformChannel = MethodChannel('example.com/plugin_example');
```

这个字符串可以是任何内容：你可以使用真实域名、虚构域名作为前缀，或者完全不使用域名，但你应该尽可能使用真实域名作为前缀，以确保它不会与其他任何插件的通道名称冲突。在这方面，它与 Java 包名非常相似，只不过在这种情况下域名前缀是可选的。

选择好通道名称后，实现包的实际接口：

`packages/making_plugins/lib/plugin_example.dart`

```dart
static Future<type> exampleMethod() async {
  return await _platformChannel.invokeMethod('runExampleMethod');
}
```

在 `Future<type>` 中，你需要用希望函数返回的类型替换 `type`。

有关 Java 和 Swift 中的哪些数据类型与 Dart 数据类型相对应的更多信息，请参见*原生 Java/Apple 与 Dart 数据类型之间的转换*表。

这就是你的包的用户实际会调用的方法，唯一需要记住的是，你调用平台特定代码的方式是通过 `platformChannel.invokeMethod(METHOD_NAME)` 来调用方法。

显然，对于这样一个简单的包，你可以跳过定义 platformChannel 变量，直接使用 `MethodChannel(CHANNEL_NAME).invokeMethod(METHOD_NAME)`，但这不会使代码简短多少，而且从长远来看这可能是一种非常糟糕的做法，所以请像我们在示例中展示的那样，事先声明通道。

### 在 Java 中实现 Android 接口

在 `android` 目录中，会有一个看起来与应用源码树完全相同的源码树，唯一的区别是文件的实际内容。

一个针对 Android 的 Java 插件实现如下：

`packages/making_plugins/android/src/main/java/com/example/pluginexample/PluginExamplePlugin.java`

```java
package com.example.pluginexample;

import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugin.common.MethodChannel.Result;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.PluginRegistry.Registrar;

public class PluginExample implements MethodCallHandler {
  public static void registerWith(Registrar registrar) {
    final MethodChannel channel =
        new MethodChannel(registrar.messenger(),
        "example.com/plugin_example");
    channel.setMethodCallHandler(new PluginExample());
  }

  @Override
  public void onMethodCall(MethodCall call, Result result) {
    if (call.method.equals("runExampleMethod")) {
      result.success(DATA_TO_RESPOND_WITH);
    } else {
      result.notImplemented();
    }
  }
}
```

具体来说，Java 文件将是一个实现了 `MethodCallHandler` 接口的类，我们需要为它定义两个方法：一个 `registerWith` 方法，用于定义要监听的通道；一个 `onMethodCall` 方法，在通道上调用方法时被调用。

