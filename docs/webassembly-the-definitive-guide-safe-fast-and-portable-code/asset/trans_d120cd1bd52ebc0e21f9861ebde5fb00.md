

![](img/16b7990eb0dbf589db177bdbd2f4a189_213_0.png)

Figure 10-2. Calling JavaScript from Rust from JavaScript

尽管结果看似平凡，但这是一个令人非常满意的结果，因为在这两种语言之间生成的桥接从任一侧看都非常自然。只是在 Rust 中思考字符串切片要容易得多，而不是像我们之前必须做的那样将字节写入内存实例。

好了，现在基础部分已经掌握，让我们尝试一些更有趣的东西。我非常喜欢 Rust 的一个特性就是它的模式匹配支持。其他语言也有这个功能，但我真的很喜欢 Rust 的实现方式。请查看示例 10-12。你会首先看到一个块上带有 #[wasm_bindgen] 属性，表示我们希望从 JavaScript 调用一个名为 log() 的方法。注意带有 js_namespace 为 console 的第二个内部属性。这展示了我们如何借助 wasm-bindgen 直接从 Rust 调用 console.log()。

## Example 10-12. Rust Pattern Matching in Action

```
use wasm_bindgen::prelude::*;
```

```
#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console)]
    fn log(s: &str);
}

#[wasm_bindgen]
pub fn describe_location( lat : f32, lon : f32 ) {
    let i_lat = lat as i32;
    let i_lon = lon as i32;

    use std::cmp::Ordering::*;

    let relative_position = match(i_lat.cmp(&38), i_lon.cmp(&-121)) {
        (Equal, Equal) => "very close!",
        (Equal, Greater) => "east of me",
        (Equal, Less) => "west of me",
        (Less, Equal) => "south of me",
        (Less, Greater) => "southeast of me",
        (Less, Less) => "southwest of me",
        (Greater, Equal) => "north of me",
        (Greater, Greater) => "northeast of me",
        (Greater, Less) => "northwest of me"
    };

    log(&format!("You are {}!", relative_position));
}
```

在 Rust 中可以使用 log() 方法后，我们定义了一个名为 describe_location() 的函数，它接受两个 f32 用于与我家的大致位置进行比较。为了简化比较并且不泄露太多关于我所在位置的细节，我只比较我当前位置的整数部分（38N，-121W）。为此，我将传入的浮点数强制转换为整数，然后导入允许我比较整数的功能。截断后的 i_lat 值与我的纬度进行比较，i_lon 值与我的经度进行比较。结果被放入一个 Rust 元组中，这类似于 Python 元组，是将一个或多个值放入单个结构中的轻量级方法。

然后元组中的值与将另一个位置与我的位置进行比较时的各种可能性进行匹配。如果 cmp() 返回两个 Equal 值，则该位置在我附近。如果纬度相等但经度不相等，则另一个位置在我东侧或西侧。我们在这里拥有的是处理九种不同情况的一种极其紧凑但可读性很强的方式。如果这被表达为一堆嵌套的 if-then 子句，可读性会困难得多。

一旦生成了相对位置的描述，我们就会调用 log 函数来打印结果。由于这最终是 console.log() 函数，结果将打印在浏览器开发者控制台中。

下一步是构建我们的包以在 HTML 和 JavaScript 中导入。

```
brian@tweezer ~/s/geo-example> wasm-pack build --target web
[INFO]: Checking for the Wasm target...
[INFO]: Compiling to Wasm...
    Compiling geo-example v0.1.0 (/Users/brian/src/geo-example)
    Finished release [optimized] target(s) in 0.51s
[INFO]: Optimizing wasm binaries with `wasm-opt`...
[INFO]: Done in 1.01s
[INFO]: Your wasm pkg is ready to publish at
/Users/brian/src/geo-example/pkg.
```

在示例 10-13 中，我们看到用于调用我们行为的 HTML 和 JavaScript。我们导入函数然后调用一个 JavaScript 函数来测试浏览器上是否可用 geolocation 对象。如果存在，我们请求当前位置，这会触发用户批准的弹出窗口。如果获得位置，结果将显示为占位段落元素的 innerText，并且我们调用 Rust 中的 describe_location() 方法进行模式匹配。

## Example 10-13. Calling the Rust pattern matching from HTML

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>parsing-example</title>
  </head>
  <body>
    <script type="module">
      import init, {describe_location} from "./pkg/parsing_example.js";
```

```
init()
    .then(() => {
        getLocation();
    });

var output = document.getElementById("output");

function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
    } else {
        output.innerHTML = "This browser doesn't support Geolocation.";
    }
}

function showPosition(position) {
    output.innerHTML = "Your position:" +
        "<br>Latitude: " + position.coords.latitude +
        "<br>Longitude: " + position.coords.longitude;

    describe_location(position.coords.latitude,
    position.coords.longitude);
}
</script>
<p>Open up your JavaScript console and allow the browser to see your location</p>
<p id="output"></p>
</body>
</html>
```

执行此代码的结果如图 10-3 所示。

![](img/16b7990eb0dbf589db177bdbd2f4a189_217_0.png)

Figure 10-3. Pattern matching geolocations from JavaScript in Rust

## Designing Code For In and Out of the Browser

我想介绍的最后一个部分是另一个我认为会越来越流行的 WebAssembly 用例。我们已经研究了使用 JavaScript 以外的语言来定位浏览器。我们也讨论了在浏览器运行时中重用现有代码的场景。但如果代码本身就是设计为在浏览器内外运行呢？我认为一旦更多人熟悉 WebAssembly 所提供的内容，这将成为一个越来越常见的场景。

只要我关注 WebAssembly，我就一直期待这种活动，但我对 Emil Ernerfeldt 的 egui 库⁷ 进展得有多远感到惊讶。他将该项目描述为“纯 Rust 中的易于使用的即时模式 GUI”⁸ 基本上，它是一个复杂的用户界面库，可在浏览器内外使用。

我们在本章中多次提到，Rust 得益于其 LLVM  heritage，可以发出各种后端目标。Emil 也正是利用这一点让它发挥作用。但他做得非常优雅，我认为可以从他所做的事情中学到很多。他实现细节的完整内容超出了本章范围，但如果您有兴趣做类似的事情，我想引起您对该项目的注意。

首先，让我们看看他的应用作为原生应用程序运行的效果。如果您使用 Linux，您必须安装 GitHub 站点上列出的几个额外包，但之后，您只需运行：

```
brian@tweezer ~/g/egui> cargo run --release -p egui_demo_app
```

结果应该看起来像图 10-4 中显示的那样，具体取决于您点击的选项。如果您尝试演示，您会发现它是一个外观吸引人、功能丰富的用户界面库。Rust 社区非常期待有更多像这样的工具包可用于构建应用程序和游戏。

![](img/16b7990eb0dbf589db177bdbd2f4a189_219_0.png)

Figure 10-4. Native execution of the egui Demo App

当您运行上述演示时执行的代码（src/main.rs）如示例 10-14 所示（我删除了一些配置代码以抑制警告）。注意 main() 方法仅在原生编译时使用。

## Example 10-14. Main program for the egui Demo App

```
// When compiling natively:
fn main() {
    let app = egui_demo_lib::WrapApp::default();
    let options = eframe::NativeOptions {
        // Let's show off that we support transparent windows
        transparent: true,
        ..Default::default()
    };
    eframe::run_native(Box::new(app), options);
}

