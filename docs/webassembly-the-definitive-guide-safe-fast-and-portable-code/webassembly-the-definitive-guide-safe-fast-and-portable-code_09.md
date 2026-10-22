## 第 8 章. 服务器上的 WebAssembly

### 早期发布读者须知

使用 Early Release 电子书，您可以在这些标题正式发布之前很久就利用这些技术——作者在撰写时对其原始未编辑内容进行获取。

这将是最终书籍的第 8 章。

如果您对我们如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 我作为焦点、处于聚光灯下时感到极度不舒服。我真的很喜欢幕后角色，因为我的自由都在那里。
> ——布莱恩·伊诺

我的职业生涯始于用户界面领域。我最初在一款用于控制网络矩阵交换机的 X/Motif¹ 应用程序上工作。从那里，我进入了一个能够显示数 TB 地形数据和超光谱图像的 Whole Earth 可视化环境。不仅从 3D 可视化角度来看很有趣，而且我们在“四人帮”《设计模式》一书问世前一年，受硅图公司的 Doug Young 启发，将整个应用程序围绕命令模式² 构建。

在大多数时候，为人类使用而编写软件是一项有趣且有回报的体验。你可以通过对他们如何完成工作任务进行一些思考，真正让人们的生活更轻松、压力更小。尽管有这些积极因素，它也有其缺点。

虽然每个人对用户界面决策都有意见，但只有其中一些是有根据的意见。

我想象在某种程度上，这就是布莱恩·伊诺在上面引文中所指的。他是 Roxy Music 的联合创始人之一，是音乐发烧友心中的宠儿。但尽管他有华丽的服装和妆容，他并不真的想站在聚光灯下，并且在与布莱恩·费里频繁冲突后，他另辟蹊径，更专注于作曲和制作，而不是成为典型的摇滚明星表演者。

我的职业生涯发生了转变，我开始专注于设计、架构、后端服务等。我远离了用户界面的聚光灯，享受后端服务器工作的相对自由。它没有面向用户的活动那么引人注目，但它也有好处，那就是没有面向用户的活动那么引人注目。

在绝大多数存在时间里，WebAssembly 被定位为客户端技术。它主要被视为扩展和拓展浏览器中可能性的方式，一个不再仅通过 JavaScript 扩展的通用客户端。WebAssembly 的作用远不止于成为前端宠儿。它在浏览器之外作为技术也将发挥极其重要的作用。事实上，它一直为此而定位，Node.js 在几乎整个时间里都支持 WebAssembly 模块。

在服务器上性能至关重要且你通常可以自由选择实现技术的情况下，这为何同样有意义可能并不明显。然而，在硬件异构性和演进、开发者生产力、业务价值、安全性和架构作为设计选择以最小化基础设施和网络成本的能力的交汇点上，却存在一个光芒四射的机会，WebAssembly 正在迅速扩展以填补这一空白。在本书的剩余部分，我们将填补这个故事，但现在我们将仅关注在浏览器之外运行 WebAssembly 的基础知识。

### 对 Node.js 的原生扩展

Node.js 出现的原因之一是，开发者在浏览器客户端使用一种语言和一组框架，而在服务器端使用另一套。之前通过 Java 本身和 Google Web Toolkit (GWT) 实现代码在各处重用的努力，曾试图从服务器到客户端的方向进行。这是一个相反方向的运动。软件开发领域的大多数兴奋点都发生在浏览器中，从 Ajax 到 jQuery 到 Angular 等等技术和框架的爆炸式增长。不得不编写在浏览器中运行的代码，然后用另一种语言重写以在服务器上运行，这令人抓狂。

Node.js 迅速流行起来，成为软件开发者的宠儿，他们现在拥有了更广的覆盖范围和更大的可重用性。作为一个基于 JavaScript 的环境，编写在上面运行的应用程序只要环境可移植就本质上是可移植的。其核心是来自 Google 的高性能 V8 JavaScript 引擎、libuv，即其事件循环的基础和对低级功能抽象层，以及建立在所有这些之上的 API 集合。这是一个本质上可移植的环境。

问题仍然存在，并非所有东西都适合用 JavaScript 实现，即使其核心是功能强大的 V8 引擎。它自然允许使用原生库扩展机制，原生库用 C 和 C++ 实现。鉴于 JavaScript 对象生命周期与原生代码结构之间的复杂关系，这使得软件开发变得更加棘手。更不用说，你突然有了原生库管理问题。如果安装了一个带有原生库扩展的 Node.js 应用程序，就需要一个流程让它们在 Linux、Windows、macOS 等系统上编译。

许多 WebAssembly 教程演示了将两个数字相加。显然，除非你在谈论机器学习级别的数学，否则这不会是一个采用原生库的好用例。在第 9 章中，我们将更深入地讨论这种场景。现在，我们将仅突出即使对于这个简单且理由不充分的示例，将 Node.js 与原生库集成也是相对复杂的。





问题在很大程度上在于 C 和 C++ 代码可以直接访问内存，而 JavaScript 代码则不能。在 Node.js 环境中，V8 引擎为在其上运行的 JavaScript 代码管理内存。这使得在 JavaScript 部分和引擎的原生部分之间传递字符串、结构、参数以及占用内存空间的其他元素变得更加棘手。V8 的设计初衷是让 JavaScript 与浏览器中一个页面上的代码无法干扰分配给不同页面上其他代码的内存。这种隔离在嵌入 Node.js 环境中时仍然保持。

服务器端框架通常是可扩展的，因此我们可以添加额外的响应行为、过滤器、授权模型和数据处理工作流。在 Java 世界中，服务器开发者可以部署行为的方式有一系列选择。有 servlet、Spring bean、响应式系统等等。这些扩展的结构通常由标准或成熟的约定定义。

在 Node.js 中，历史上一直有 Express 等中间件，然后是用 C 和 C++ 编写的原生插件。该环境中的大多数应用程序都没有原生插件，因为 JavaScript 引擎已经变得相当高效，而且有似乎无穷无尽的开源库来解决各种需求。然而，在 JavaScript 性能不合适的情况下，仍然可以创建一个扩展并使其可从 JavaScript 端调用。

不幸的是，这并不是一项简单的活动。首先，许多 JavaScript 开发者并不是熟练的 C 和 C++ 程序员。两种语言和运行时之间存在很大差异，而且在不受限制的 C 和 C++ 世界与隔离的、垃圾回收的 JavaScript 世界之间来回传递内存需要做大量的额外工作。即使开发者在这些低级语言中也很有效，原生库的采用也会使构建过程变得复杂。突然间，程序工件不再是固有的可移植的，我们需要跟踪原生库的 Linux、macOS 和 Windows 版本。

让我们来看一个我们在本书中多次看到的简单示例。Node.js 关于插件的文档为我们提供了一个很好的示例来做比较³。首先，确保 node 和 node-gyp 命令已按附录 A 安装。然后，查看示例 8-1，你会看到一个将两个数字相加的单个函数。

#### 示例 8-1. 来自 Node 网站的 Node.js 插件

```
// addon.cc
// Taken from
https://nodejs.org/api/addons.html#addons_addon_examples

#include <node.h>

namespace demo {

using v8::Exception;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Number;
using v8::Object;
using v8::String;
using v8::Value;

// This is the implementation of the "add" method
// Input arguments are passed using the
// const FunctionCallbackInfo<Value>& args struct
void Add(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  // Check the number of arguments passed.
  if (args.Length() < 2) {
    // Throw an Error that is passed back to JavaScript
    isolate->ThrowException(Exception::TypeError(
      String::NewFromUtf8(isolate,
        "Wrong number of arguments").ToLocalChecked()));
    return;
  }

  // Check the argument types
  if (!args[0]->IsNumber() || !args[1]->IsNumber()) {
    isolate->ThrowException(Exception::TypeError(
      String::NewFromUtf8(isolate,
        "Wrong arguments").ToLocalChecked()));
return;
}

// Perform the operation
double value =
    args[0].As<Number>()->Value() + args[1].As<Number>()->Value();
    Local<Number> num = Number::New(isolate, value);

// Set the return value (using the passed in
// FunctionCallbackInfo<Value>&)
args.GetReturnValue().Set(num);
}

void Init(Local<Object> exports) {
    NODE_SET_METHOD(exports, "add", Add);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Init)

}  // namespace demo
```

该方法称为 Add()，它接受一个 FunctionCallbackInfo<Value>& 引用。从中，我们检索 Isolate 实例，即我们对 V8 为此实例维护的内存子系统的句柄。如果参数不是两个或不是数值类型，我们会抛出异常。否则，我们将值作为数字检索并相加，然后创建一个新位置来保存该值，然后将其设置为函数的返回类型。此外，我们还需要在 Init() 方法中通过 Node.js 注册该模块。

下一步是构建插件。在示例 8-2 中，你可以看到 binding.gyp 文件，它指示 node-gyp 命令如何执行构建。这可以是一个更详细的过程，但我们的需求在这里相当简单。

#### 示例 8-2. 插件的构建说明

```
{
  "targets": [
    {
      "target_name": "addon",
      "sources": [ "addon.cc" ]
    }
  ]
}
```
```
}
]
}
```

构建命令相当简单明了（我隐藏了一些细节）：

```
brian@tweezer ~/g/w/s/c/node-addon> node-gyp configure build
gyp info it worked if it ends with ok
gyp info using node-gyp@8.0.0
gyp info using node@15.4.0 | darwin | x64
gyp info find Python using Python version 3.8.3 found at
"/usr/local/bin/python3"
gyp http GET https://nodejs.org/download/release/v15.4.0/node-v15.4.0-headers.tar.gz
gyp http 200 https://nodejs.org/download/release/v15.4.0/node-v15.4.0-headers.tar.gz
gyp http GET
https://nodejs.org/download/release/v15.4.0/SHASUMS256.txt
gyp http 200
https://nodejs.org/download/release/v15.4.0/SHASUMS256.txt
...
gyp info spawn args [ 'BUILDTYPE=Release', '-C', 'build' ]
  CXX(target) Release/obj.target/addon/addon.o
  SOLINK_MODULE(target) Release/addon.node
```

此时，插件已构建完成。我们可以用示例 8-3 所示的代码对其进行测试。

#### 示例 8-3. 用于测试原生插件的 JavaScript 代码

```
// test.js
const addon = require('./build/Release/addon');

console.log('This should be eight:', addon.add(3, 5));
```

表面上，使用插件的感觉与调用 WebAssembly 模块非常相似，但显然其实现比我们之前调用的用于添加两个数字的 C 代码复杂得多。真正的问题是，原生库的复杂性管理起来有点麻烦。现在，有了 WebAssembly，就不再需要这样做，你可以理解为什么 Node.js 社区对库的 WebAssembly 版本感到兴奋

关于 WebAssembly 版本的库。它们提供了良好的性能提升，但完全可移植并简化了部署模型。

### WebAssembly 和 Node.js

虽然大多数人认为 WebAssembly 是客户端浏览器技术，但 Node.js 在很久以前就已经支持从 WebAssembly 模块调用函数，几乎与浏览器同时。 我不会创建任何实际的“服务器”，因为我认为这可能会分散我想传达的重点，但你显然可以设置一个 REST API 或类似的东西并仍然使用我提到的功能。

让我们从示例 8-4 中的一个简单示例开始。

示例 8-4. 一个简单的程序来添加两个数字

```
#include <stdio.h>

int add(int x, int y) {
    return x + y;
}

int main() {
    printf("The sum of 2 and 3 is: %d\n", add(2,3));
    return 0;
}
```

使用 clang，这可以非常简单地编译和执行。

```
brian@tweezer ~/g/w/s/c/node> clang add.c
brian@tweezer ~/g/w/s/c/node> ./a.out
The sum of 2 and 3 is: 5
```

如果我们更新源代码以包含一些 Emscripten 相关的宏，我们就可以像之前看到的那样在 Node.js 中运行它。我还移除了 main() 方法，这样我们的模块将不再期望 printf() 函数的实现，因为我们将在服务器端 JavaScript 的世界中运行。更新后的代码在示例 8-5 中。

#### 示例 8-5. 使用 Emscripten 宏的简单程序来添加两个数字

```
#include <emscripten.h>

EMSCRIPTEN_KEEPALIVE int add(int x, int y) {
    return x + y;
}
```

> 1  包含 Emscripten 头文件以获取宏定义。
> 2  告诉 Emscripten 编译器保留 add() 方法。

现在我们可以用 emcc 重新编译并用 Node.js 运行它：

```
brian@tweezer ~/g/w/s/c/node> emcc add.c
brian@tweezer ~/g/w/s/c/node> ls -alF
total 376
drwxr-xr-x   6 brian  staff       192 Apr 18 15:05 ./
drwxr-xr-x  10 brian  staff       320 Apr 18 13:08 ../
-rwxr-xr-x   1 brian  staff     49456 Apr 18 15:05 a.out*
-rw-r--r--   1 brian  staff    121686 Apr 18 15:05 a.out.js
-rwxr-xr-x   1 brian  staff     11805 Apr 18 15:05 a.out.wasm*
-rw-r--r--   1 brian  staff       141 Apr 18 15:05 add.c
brian@tweezer ~/g/w/s/c/node> node a.out.js
The sum of 2 and 3 is: 5
```

如果你调查 a.out.js 文件，你会看到 Emscripten 工具链为我们处理的所有设置。

Node.js 运行时提供了适当的基于 JavaScript 的 WebAssembly API，如我们在浏览器中看到的那样。这允许你加载和实例化模块并按需调用其行为。在幕后，这就是 Emscripten 工具链为我们生成的代码。

然而，我们也对简化服务器中 WebAssembly 模块的加载和实例化感兴趣，就像在浏览器中一样。Node.js 提供了实验性的支持，可将它们作为 ES6 模块加载，如示例 8-6 所示。

#### 示例 8-6. 将 WebAssembly 模块作为 ES6 模块加载





```
import * as Module from './a.out.wasm';

console.log('The sum of 6 and 2 is: ' + Module.add(6,2));
```

根据运行的时间，你可能需要实验性功能标志，但请注意，与我们之前看到的情况相比，使用起来容易得多。行为的表达也比我们在之前关于原生插件的讨论中看到的简单得多。你可以看到为什么这个社区对随着时间的推移不断增加的 WebAssembly 支持感到兴奋。

```
brian@tweezer ~/g/w/s/c/node> node --experimental-modules ↵
  --experimental-wasm-modules index.mjs
(node:74571) ExperimentalWarning: Importing Web Assembly modules
is an
experimental feature. This feature could change at any time (Use
`node
--trace-warnings ...` to show where the warning was created)
The sum of 6 and 2 is: 8
```

作为最后一个例子，我想引入一个更复杂的第三方库。出于我将在本章末尾解释的原因，找到一个不会打开太多潘多拉魔盒的良好示例很棘手。我们还没有介绍一些东西，但本章开始为此奠定了基础。

### 供应链攻击

这就引出了另一个考虑。我们在安全软件系统世界中面临一个非常严重的问题，称为供应链攻击。这不是一个新问题，但它变得越来越严重和频繁。

没有单一的方法可以构建一个安全系统， certainly 不是简单地通过打开加密或某些安全功能来实现。那些对于安全系统可能是必要的，但绝对是不够的。通常通过纵深防御⁴、最小权限原则⁵ 以及有意地在组织层面承担安全责任的结合，你才能开始朝着正确的方向前进。

对我们来说，问题在于我们正在运行来自不受信任的第三方的不受信任的代码，并赋予了我们在生产环境中通常给予自己的权限。开箱即用，Node 无法对此提供任何保护，这是一个严重的问题，正在为网络钓鱼、数据泄露和其他攻击打开新的攻击向量。

这里有一个此类攻击的撰写⁶。基本思路是，攻击者会制作一个适度有用的开源功能片段，让它开始被使用。开发人员经常在不考虑其来源或依赖项的传递来源集合的情况下添加依赖项。一旦代码在生态系统中获得足够的使用量，在精心控制的情况下进行的小幅更新开始以微妙且难以预料的方式暴露攻击。基本上，代码可能会开始寻找加密货币私钥或其他有用的敏感信息。

解决这个问题的唯一真正解决方案之一涉及一个积极且细心的软件开发人员社区，手动检查每个依赖项的每次更新（及其相应的全面的传递依赖列表），这几乎可以保证永远不会发生。另一个解决方案是在任意代码不被赋予为所欲为的权限的场景下运行。Node.js 传统上允许这种模型，这也是其创始人 Ryan Dahl 另起炉灶创建更安全的新东西的原因之一。

进入 Deno。

### WebAssembly 和 Deno

Deno⁷ 基本上是比 Node.js 更安全的 JavaScript 和 TypeScript 运行时，即使两者最初都是由同一个人构建的。与 Node.js 相比，安全性并不是那么大的考虑因素，因此事后很难附加。Deno 从一开始就将安全作为默认立场。在 Deno 运行时中运行的代码如果未被授予权限，则无法访问文件系统或打开网络连接。

这显然不是一个新想法。Java 几乎在其存在的整个时间内就一直在其核心拥有安全权限模型。问题在于 Java 的权限模型可能有点繁琐且难以正确处理。如果有什么东西会扼杀安全性，复杂性名列前茅。正如你将在下面看到的，Deno 使用基于能力⁸ 的方法以更简单的方式处理这个问题。

除了安全性之外，Deno “原生运行 TypeScript”，而通常需要先将其转译为某种 JavaScript。虽然 Deno 在幕后编译它并缓存编译后的形式，但感觉更像是原生支持。这通过允许改进的类型检查来改善 JavaScript 开发的质量（这也具有安全影响），因为由于 TypeScript 中强大的类型系统，运行时通常会出现的问题可以在编译时被检测到。

让我们从 Example 8-5 中添加两个数字的 WebAssembly 模块开始。在 Example 8-7 中，你可以看到我们第一次尝试使用 Deno 的 WebAssembly 支持。超级简单！

#### Example 8-7. 在 Deno 中加载 WebAssembly 模块

```
const wasmCode = await Deno.readFile("./a.out.wasm");
const wasmModule = new WebAssembly.Module(wasmCode);
const wasmInstance = new WebAssembly.Instance(wasmModule);
const add = wasmInstance.exports.add as CallableFunction

console.log("2 + 3 = " + add(2,3));
```

不幸的是，我们的兴奋很快就结束了。至少问题很清楚。我们试图从文件系统读取，但没有权限这样做。我们立即看到了 Deno 的安全优势。

```
brian@tweezer ~/g/w/s/c/deno> deno run main.ts
error: Uncaught (in promise) PermissionDenied: Requires read access to "./a.out.wasm",
run again with the --allow-read flag
```

```
const wasmCode = await Deno.readFile("./a.out.wasm");
^
    at unwrapOpResult (deno:core/core.js:100:13)
    at async open (deno:runtime/js/40_files.js:46:17)
    at async Object.readFile
(deno:runtime/js/40_read_file.js:19:18)
    at async file:///Users/brian/git-personal/webassembly-the-definitive-guide/src/ch08/deno/main.ts:1:18
```

如果我们使用以下命令重新运行，事情会愉快得多。

```
brian@tweezer ~/g/w/s/c/deno> deno run --allow-read main.ts
Check file:///Users/brian/git-personal/webassembly-the-definitive-guide/src/ch08/deno/main.ts
2 + 3 =  5
```

虽然我不想太分散地讨论运行 Node.js 和 Deno HTTP 服务器的细节，但我确实承认，在一章关于服务器的章节中我还没有运行过服务器，这有点可悲。所以，这里有一个简单的 HTTP 服务器。要变得更复杂就需要我们进入 Deno 中间件。

在 Example 8-8 中，你可以看到 Deno 如何允许你通过 HTTP 拉取版本化的模块供使用。在这种情况下，我们从 Deno 标准库中拉取一个基本的 HTTP 服务器。

#### Example 8-8. 在 Deno HTTP 服务器中使用 WebAssembly

```
import { serve } from "https://deno.land/std@0.93.0/http/server.ts";

const wasmCode = await Deno.readFile("./a.out.wasm");
const wasmModule = new WebAssembly.Module(wasmCode);
const wasmInstance = new WebAssembly.Instance(wasmModule);
const add = wasmInstance.exports.add as CallableFunction

const server = serve({ hostname: "0.0.0.0", port: 9000 });
console.log(`HTTP webserver running.  Access it at:
http://localhost:9000/`);

for await (const request of server) {
  let bodyContent = "2 + 3 = " + add(2,3);
  request.respond({ status: 200, body: bodyContent });
}
```

为快速失望做好准备！正如我们不能在不授予应用程序读取文件系统的权限的情况下运行读取文件系统的 TypeScript 代码一样，我们也不能在没有权限的情况下监听网络连接！

```
brian@tweezer ~/g/w/s/c/deno> deno run --allow-read main-serve.ts
error: Uncaught (in promise) PermissionDenied: Requires net
access to "0.0.0.0:9000", run again with the --allow-net flag
    const listener = Deno.listen(addr);
                    ^

    at unwrapOpResult (deno:core/core.js:100:13)
    at Object.opSync (deno:core/core.js:114:12)
    at opListen (deno:runtime/js/30_net.js:18:17)
    at Object.listen (deno:runtime/js/30_net.js:184:17)
    at serve (https://deno.land/std@0.93.0/http/server.ts:303:25)
    at file:///Users/brian/git-personal/webassembly-the-
definitive-guide/src/ch08/deno/main-serve.ts:8:16
```

幸运的是，我们被告知该怎么做，而且这是一个很容易修复的问题。

```
brian@tweezer ~/g/w/s/c/deno> deno run --allow-read --allow-net main-serve.ts
HTTP webserver running. Access it at: http://localhost:9000/
```

现在一个简单的 HTTP 客户端可以获取我们的结果。

```
brian@tweezer ~> http http://localhost:9000
HTTP/1.1 200 OK
content-length: 9

2 + 3 = 5
```

现在我们将看一个 Deno 和 WebAssembly 的最后一个例子。我一直有点谨慎地展示某些类型的功能，直到我们有机会在第 11 章讨论 WebAssembly 服务接口 WASI 标准。我保证在那时我们会查看更有趣的 Node.js 和 Deno 示例。目前我想展示一个使用 WebAssembly 与 Deno 的用法，不需要太多额外的细节。





Tilman Roeder⁹ 创建了一个内存中的 SQLite WebAssembly 模块，并将其封装以供 JavaScript 和 TypeScript¹⁰ 使用。它的工作原理细节稍后再说，但使用起来相当直接，如示例 8-9 所示。

#### Example 8-9. Using a WebAssembly SQLite wrapper in a Deno HTTP Server

```
import { DB } from "https://deno.land/x/sqlite/mod.ts";
import { serve } from "https://deno.land/std@0.93.0/http/server.ts";

// Create the Database. This requires write access!

const db = new DB("pl.db");
db.query(
  "CREATE TABLE IF NOT EXISTS languages (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT)",
);

const names = ["C", "C++", "Rust", "TypeScript"]

// Populate the database

for (const name of names) {
  db.query("INSERT INTO languages (name) VALUES (?)", [name]);
}

// Close out the connection

db.close();

const server = serve({ hostname: "0.0.0.0", port: 9000 });
console.log(`HTTP webserver running. Access it at: http://localhost:9000/`);

for await (const request of server) {
  // Re-open the Database

  const db = new DB("pl.db");
  let bodyContent = "Programming Languages that work with WebAssembly:\n\n";

  for(const [name] of db.query("SELECT name FROM languages")) {
    bodyContent += name + "\n";
  }
  
bodyContent += "\n";
request.respond({ status: 200, body: bodyContent });

// Close the Database
db.close();
}
```

我们首先创建数据库文件。我希望你的直觉告诉你这将需要另一个运行时权限，因为确实如此，如下所示。之后，我们将一些数据加载到数据库中并关闭连接。

一旦服务器启动，在收到合适的 HTTP 请求后，我们将再次打开数据库，运行查询，生成结果然后关闭数据库。我并不是说这已经是高质量的生产代码，但令人惊讶的是，这段代码能够在 Deno 中安全运行，并且将在 Deno 支持的各种平台上运行。即使我们处理的是一个封装的 C 库（即 SQLite3），WebAssembly 也使代码具有可移植性，同时性能仍然相当不错。我希望通过安全、快速且可移植的 WebAssembly 代码扩展服务器基础设施的想法变得更加清晰。

以下命令将使用合适的权限启动服务器。

```
brian@tweezer ~/g/w/s/c/deno> deno run --allow-read --allow-write
--allow-net db-serve.ts
HTTP webserver running. Access it at: http://localhost:9000/
```

来自 HTTP 客户端的请求会产生我们所期望的结果。

```
brian@tweezer ~> http http://localhost:9000
HTTP/1.1 200 OK
content-length: 74

Programming Languages that work with WebAssembly:

C
C++
Rust
TypeScript
```

### A Look Forward

尽管本章在浏览器之外迈出了巨大一步，但这只是开始。虽然我们放弃了在受限环境中运行的安全限制（除了同样受限的 Deno 实例），但我们也放弃了浏览器的丰富功能。任何现代浏览器平台的 JavaScript 环境都提供了大量功能。这包括 JavaScript 引擎、硬件加速的 2D/3D 绘图和视频播放、声音、字体支持、进行网络请求的能力等等。默认情况下，Node.js 或 Deno 都不提供浏览器的全部功能，尽管 Deno 正在努力支持其中大部分功能。这使得编写可在浏览器内外运行的 WebAssembly 应用程序变得更加困难。

WebAssembly 使代码具有可移植性。我们需要另一种策略，通过为我们期望在现代计算平台上提供的功能提供一致的服务接口，使应用程序具有可移植性。这就是为什么我对我能向你展示的示例类型有些谨慎。解决这个问题的真正方案将在第 11 章中介绍。在此之前，请耐心等待，但在我们前往的路上还有很多要讨论的内容。

- 1 Motif built on top of the Xt Intrinsics libraries and the X Window System:
  https://en.wikipedia.org/wiki/Motif_(software)
  
- 2 The Command Pattern separates the triggering event for execution of code from the actual code allowing for, among other things, macro recording and playback:
  https://en.wikipedia.org/wiki/Command_pattern
  
- 3 There are several examples of building Node addons here:
  https://nodejs.org/api/addons.html#addons_addon_examples
  
- 4 A combination of overlapping security controls can help you protect against unexpected vulnerabilities: https://en.wikipedia.org/wiki/Defense_in_depth_(computing)
  
- 5 Only assigning the minimum privileges necessary to users is part of how this technique helps. More information can be found here:
  https://en.wikipedia.org/wiki/Principle_of_least_privilege
  
- 6 A description of a Supply Chain Attack from 2018: https://medium.com/@hkparker/analysis-of-a-supply-chain-attack-2bd8fa8286ac

- 7 There is a lot to like about Deno beyond security. Check it out: https://deno.land.

- 8 Capability-based security systems generally provide actors to demonstrate that they have an unforgeable permission to conduct an operation. https://en.wikipedia.org/wiki/Capability-based_security

- 9 @dyedgreen on Twitter and Github

- 10 You can access the GitHub repository for this project here:
  https://github.com/dyedgreen/deno-sqlite
