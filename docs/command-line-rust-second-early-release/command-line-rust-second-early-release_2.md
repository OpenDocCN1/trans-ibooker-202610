# 第 2 章. 测试回显

## 早期发布读者的说明

使用早期发布的电子书，你可以在这些标题正式发布之前很久就利用这些技术。

这将是最终书籍的第 2 章。请注意 GitHub 仓库稍后才会激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者在本章中发现缺失的材料，请联系作者 kyClark@gmail.com。

> *到你收到这条说明时，我们将不再活着，我们都将化为烟雾，无法回复。*
> 
> —They Might Be Giants

在第 1 章中，你编写了始终以相同方式运行的程序。在本章中，你将学习如何使用用户的参数在运行时更改程序的行为。本章中你将编写的挑战程序是 `echo` 的克隆，它将打印命令行上的参数，可选择以换行符结尾。

在本章中，你将学习：

- 如何使用 `clap` crate 处理命令行参数
- 关于 `STDOUT` 和 `STDERR`
- 关于 Rust 类型如字符串、向量、切片和单元类型
- 如何使用 `match`、`if` 和 `return` 等表达式
- 如何使用 `Option` 表示可选值
- 如何使用 `Result` 的 `Ok` 和 `Err` 变体处理错误
- 如何退出程序并发出成功或失败信号
- 堆栈内存和堆内存之间的区别

## 使用 Cargo 启动新的二进制程序

这个挑战程序将被称为 `echor`，即 `echo` 加上 Rust 的 `r`。（我无法决定我是像 eh-core 还是 eh-koh-ar 发音。）我希望你已经创建了一个目录来存放你在本书中编写的所有项目。进入该目录并使用 Cargo 启动你的 *crate*，它是一个 Rust 二进制程序或库：

```
$ cargo new echor
    Created binary (application) `echor` package
```

你应该看到熟悉的目录结构：

```
$ tree echor/
echor/
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

进入新的 *echor* 目录并使用 Cargo 运行程序：

```
$ cd echor
$ cargo run
```

Hello, world! ①

① 默认程序总是打印“Hello, world！”

你已经在第 1 章中看到过这个程序，但我想指出代码的另外几点：

```
$ cat src/main.rs
fn main() {
    println!("Hello, world!");
}
```

正如你在第 1 章中看到的，Rust 将通过执行 src/main.rs 中的 main 函数来启动程序。函数的任何参数都包含在函数名后的括号中，因此这里的空括号表示函数不接受参数。所有函数都返回值，返回类型可以用箭头和类型表示，例如 -> u32 表示函数返回无符号 32 位整数。main 没有任何返回类型意味着函数返回 Rust 所谓的单元类型。请注意 println! 宏会自动在输出后附加换行符，这是一个我需要控制的功能，当用户请求不以换行符结尾时。

## NOTE

单元类型就像一个空值，用一对空括号 () 表示。文档说它“用于在没有其他有意义的值可以返回时”。它不完全像其他语言中的空指针或未定义值，这一概念最初由 Tony Hoare 提出（与 Rust 创建者 Graydon Hoare 无亲属关系），他称空引用为他的“十亿美元错误”。由于 Rust 通常不允许你取消引用空指针，因此它在逻辑上至少值十亿美元。



## echo 如何工作

echo 程序非常简单。我将回顾它的工作原理，以便你能考虑你的版本需要的所有功能。首先，echo 会将参数打印到 STDOUT：

```
$ echo Hello
Hello
```

我使用的是 bash shell，它假设任意数量的空格分隔参数，因此包含空格的参数必须用引号括起来。在下面的命令中，我将四个词作为单个参数提供：

```
$ echo "Rust has assumed control"
Rust has assumed control
```

如果没有引号，我提供的是四个独立的参数。请注意，我在提供参数时使用了不同数量的空格，但 echo 在打印时使用单个空格分隔每个参数：

```
$ echo Rust  has assumed   control
Rust has assumed control
```

如果我想保留空格，必须将它们用引号括起来：

```
$ echo "Rust  has assumed   control"
Rust  has assumed   control
```

命令行程序响应 -h 或 --help 标志以打印程序使用方法的消息的情况非常常见，所谓的 usage，因为那通常是输出的第一个词。如果我用 echo 尝试这样做，我只会得到标志的文本：

```
$ echo --help
--help
```

要了解有关该程序的更多信息，请执行 **man echo** 阅读手册页。你会看到我使用的是 2003 年的 BSD 版本程序：

```
ECHO(1)						BSD General Commands Manual
ECHO(1)
```

### NAME
	echo -- 将参数写入标准输出

### SYNOPSIS
	echo [-n] [string ...]

### DESCRIPTION
	echo 实用程序将任何指定的操作数写入标准输出，操作数之间用单个空白
	(' ') 字符分隔，并在末尾加上换行符 ('\n')
	字符。

	以下选项可用：

	-n	不打印尾部的换行符。
	这也可以通过在字符串末尾添加 '\c' 来实现，
		就像 iBCS2 兼容系统所做的那样。 请注意，此选项以及
		'\c' 的效果在 IEEE Std 1003.1-2001
		('POSIX.1') 中是实现定义的，经 Cor. 1-2002 修订。
		旨在实现最大可移植性的应用程序强烈建议
		使用 printf(1) 来抑制换行符。

	某些 shell 可能提供一个与此实用程序相似或相同的内置 echo 命令。 最值得注意的是，sh(1) 中的内置 echo
		不接受 -n 选项。 请参阅 builtin(1) 手册页。

### EXIT STATUS

echo 实用程序在成功时退出 0，并在发生错误时退出 >0。

## SEE ALSO

builtin(1), csh(1), printf(1), sh(1)

## STANDARDS

echo 实用程序符合 IEEE Std 1003.1-2001 ('POSIX.1')，经 Cor. 1-2002 修订。

默认情况下，命令行上打印的文本以换行符结尾。如前面的手册页所示，echo 有一个 -n 选项可省略换行符。根据你使用的 echo 版本，这可能不会影响输出。例如，我使用的 BSD 版本显示如下：

```
$ echo -n Hello
Hello
$ ①
```

① BSD echo 会在下一行显示我的命令提示符 $。

而 Linux 上的 GNU 版本显示如下：

```
$ echo -n Hello
Hello$ ①
```

① GNU echo 会在 Hello 之后立即显示我的命令提示符。

无论使用哪种版本的 echo，我都可以使用 bash 重定向运算符 > 将 STDOUT 发送到文件：

```
$ echo Hello > hello
$ echo -n Hello > hello-n
```

diff 工具会显示两个文件之间的差异。此输出显示第二个文件 (hello-n) 末尾没有换行符：

```
$ diff hello hello-n
1c1
< Hello
---
> Hello
\ No newline at end of file
```

## 获取命令行参数

首先要做的是获取要打印的命令行参数。在 Rust 中，你可以使用 std::env::args 来实现。std 告诉我它在标准库中，是 Rust 代码中如此通用以至于包含在语言中的代码。env 部分告诉我这是用于与环境交互的地方，程序将在那里找到参数。如果你查看该函数的文档，你会看到它返回类型为 Args 的内容：

```
pub fn args() -> Args
```

如果你[content]
## echo 如何工作

echo 程序非常简单。我将回顾它的工作原理，以便你能考虑你的版本需要的所有功能。首先，echo 会将参数打印到 STDOUT：

```
$ echo Hello
Hello
```

我使用的是 bash shell，它假设任意数量的空格分隔参数，因此包含空格的参数必须用引号括起来。在下面的命令中，我将四个词作为单个参数提供：

```
$ echo "Rust has assumed control"
Rust has assumed control
```

如果没有引号，我提供的是四个独立的参数。请注意，我在提供参数时使用了不同数量的空格，但 echo 在打印时使用单个空格分隔每个参数：

```
$ echo Rust  has assumed   control
Rust has assumed control
```

如果我想保留空格，必须将它们用引号括起来：

```
$ echo "Rust  has assumed   control"
Rust  has assumed   control
```

命令行程序响应 -h 或 --help 标志以打印程序使用方法的消息的情况非常常见，所谓的 usage，因为那通常是输出的第一个词。如果我用 echo 尝试这样做，我只会得到标志的文本：

```
$ echo --help
--help
```

要了解有关该程序的更多信息，请执行 **man echo** 阅读手册页。你会看到我使用的是 2003 年的 BSD 版本程序：

```
ECHO(1)						BSD General Commands Manual
ECHO(1)
```

### NAME
	echo -- 将参数写入标准输出

### SYNOPSIS
	echo [-n] [string ...]

### DESCRIPTION
	echo 实用程序将任何指定的操作数写入标准输出，操作数之间用单个空白
	(' ') 字符分隔，并在末尾加上换行符 ('\n')
	字符。

	以下选项可用：

	-n	不打印尾部的换行符。
	这也可以通过在字符串末尾添加 '\c' 来实现，
		就像 iBCS2 兼容系统所做的那样。 请注意，此选项以及
		'\c' 的效果在 IEEE Std 1003.1-2001
		('POSIX.1') 中是实现定义的，经 Cor. 1-2002 修订。
		旨在实现最大可移植性的应用程序强烈建议
		使用 printf(1) 来抑制换行符。

	某些 shell 可能提供一个与此实用程序相似或相同的内置 echo 命令。 最值得注意的是，sh(1) 中的内置 echo
		不接受 -n 选项。 请参阅 builtin(1) 手册页。

### EXIT STATUS

echo 实用程序在成功时退出 0，并在发生错误时退出 >0。

## SEE ALSO

builtin(1), csh(1), printf(1), sh(1)

## STANDARDS

echo 实用程序符合 IEEE Std 1003.1-2001 ('POSIX.1')，经 Cor. 1-2002 修订。

默认情况下，命令行上打印的文本以换行符结尾。如前面的手册页所示，echo 有一个 -n 选项可省略换行符。根据你使用的 echo 版本，这可能不会影响输出。例如，我使用的 BSD 版本显示如下：

```
$ echo -n Hello
Hello
$ ①
```

① BSD echo 会在下一行显示我的命令提示符 $。

而 Linux 上的 GNU 版本显示如下：

```
$ echo -n Hello
Hello$ ①
```

① GNU echo 会在 Hello 之后立即显示我的命令提示符。

无论使用哪种版本的 echo，我都可以使用 bash 重定向运算符 > 将 STDOUT 发送到文件：

```
$ echo Hello > hello
$ echo -n Hello > hello-n
```

diff 工具会显示两个文件之间的差异。此输出显示第二个文件 (hello-n) 末尾没有换行符：

```
$ diff hello hello-n
1c1
< Hello
---
> Hello
\ No newline at end of file
```

## 获取命令行参数

首先要做的是获取要打印的命令行参数。在 Rust 中，你可以使用 std::env::args 来实现。std 告诉我它在标准库中，是 Rust 代码中如此通用以至于包含在语言中的代码。env 部分告诉我这是用于与环境交互的地方，程序将在那里找到参数。如果你查看该函数的文档，你会看到它返回类型为 Args 的内容：

```
pub fn args() -> Args
```

如果你跟随 Args 文档的链接，你会发现它是一个 struct，即 Rust 中的一种数据结构。如果你查看页面左侧，你会看到诸如 trait 实现、其他相关结构体、函数等内容。我稍后会探索这些想法，但现在，只需浏览文档并尝试吸收你所看到的内容。

编辑 src/main.rs 以打印参数。我可以通过使用完整路径后跟一对空括号来调用该函数：

```
fn main() {
    println!(std::env::args()); // This will not work
}
```

当我使用 cargo run 执行程序时，我看到以下错误：

```
$ cargo run
    Compiling echor v0.1.0 (/Users/kyclark/work/sysprog-rust/playground/echor)
error: format argument must be a string literal
  --> src/main.rs:2:14
   |
2 |     println!(std::env::args()); // This will not work
   |               ^^^^^^^^^^^^^^^^
   |
help: you might be missing a string literal to format with
   |
2 |     println!("{}", std::env::args()); // This will not work
   |
     ^^^^
```

这是我第一次与编译器交锋。它说它无法直接打印该函数返回的值，但它也告诉我如何修复问题。它希望我首先提供一个包含一对大括号 { } 的字面字符串，作为打印值的占位符，因此我相应地更改代码：

```
fn main() {
    println!("{}", std::env::args()); // This will not work either
}
```

再次运行程序，看看我们还没有脱离困境。请注意，我将省略“Compiling”和其他行以专注于重要的输出：

```
$ cargo run
error[E0277]: `Args` doesn't implement `std::fmt::Display`
  --> src/main.rs:2:20
```

```
|
2 |     println!("{}", std::env::args()); // This will not work either
|                 ^^^^^^^^^^^^^^^^ `Args` cannot be
formatted with
                        the default
formatter
|
= help: the trait `std::fmt::Display` is not implemented
for `Args`
= note: in format strings you may be able to use `{:?}`
(or {:#?} for
  pretty-print) instead
= note: required by `std::fmt::Display::fmt`
= note: this error originates in a macro (in Nightly
builds, run with
  -Z macro-backtrace for more info)
```

编译器消息中有很多信息。首先，它说 trait std::fmt::Display 未为 Args 实现。Rust 中的 trait 是一种以抽象方式定义对象行为的方法。如果对象实现了 Display trait，则可以将其格式化为“面向用户的输出”。再次查看 Args 文档的“Trait Implementations”部分，注意到，确实，Display 在那里没有提到。

编译器建议我应该对占位符使用 {:?} 而不是 {}。这是打印结构的 Debug 版本的指示，它将“以面向程序员的调试上下文格式化输出”。再次参考 Args 文档，看看 Debug 列在“Trait Implementations”下，因此可行：

```
fn main() {
    println!("{:?}", std::env::args()); // Success at last!
}
```

现在程序编译并打印了一些模糊有用的内容：

```
$ cargo run
Args { inner: ["target/debug/echor"] }
```

如果你不熟悉命令行参数，第一个值通常是程序本身的路径。它不是 *per se* 的参数，但它是有用的信息。需要注意的是，此程序已编译到路径 *target/debug/echor*。除非另有说明，否则 Cargo 会将可执行文件，也称为 *binary*，放在那里。接下来，我将传递一些参数：

```
$ cargo run Hello world
Args { inner: ["target/debug/echor", "Hello", "world"] }
```

万岁！看起来我能够获取到程序的参数。我传递了两个参数，*Hello* 和 *world*，它们显示为二进制名称之后的其他值。我知道我需要传递 *-n* 标志，所以让我试试：

```
$ cargo run Hello world -n
Args { inner: ["target/debug/echor", "Hello", "world", "-n"] }
```

通常也会将标志放在值之前，所以让我试试：

```
$ cargo run -n Hello world
error: Found argument '-n' which wasn't expected, or isn't valid in this context

USAGE:
    cargo run [OPTIONS] [--] [args]...

For more information try --help
```

这不起作用，因为 Cargo 认为 *-n* 参数是为它自己，而不是我正在运行的程序。为了解决这个问题，我需要使用两个破折号分隔 Cargo 的选项：

```
$ cargo run -- -n Hello world
Args { inner: ["target/debug/echor", "-n", "Hello", "world"] }
```



在程序参数的术语中，-n 是一个可选参数，因为你可以省略它。通常程序选项以一个或两个破折号开头。常见的做法是使用一个破折号加单个字符作为短名称，例如 -h 表示帮助标志，以及使用两个破折号加一个单词作为长名称，例如 --help。具体来说，-n 和 -h 都是标志，存在时有一种含义，不存在时则相反。在这种情况下，-n 表示省略尾部的换行符；否则，按正常方式打印。

echo 的所有其他参数都是位置参数，因为它们相对于程序名称（参数中的第一个元素）的位置决定了其含义。考虑 chmod 命令，它接受两个位置参数，第一个是类似 755 的模式，第二个是文件或目录名。在 echo 的情况下，所有位置参数都被解释为要打印的文本，并且应按给定的顺序打印。这并不是一个坏的开始，但本书中程序的参数将会变得更加复杂。我需要找到更好的方法来解析程序的参数。

## 将 clap 添加为依赖项

虽然有各种方法和 crate 用于解析命令行参数，但我将在本书中 exclusively 使用 clap crate。要开始使用，我需要告诉 Cargo 我想下载此 crate 并在项目中使用。我可以通过向 Cargo.toml 添加依赖项来实现。编辑你的文件，在 [dependencies] 部分添加 clap 版本 2.33：

```
$ cat Cargo.toml
[package]
name = "echor"
version = "0.1.0"
edition = "2018"
```

```
[dependencies]
clap = "2.33" ①
```

① crate 名称未加引号。等号表示 crate 的版本，应加引号。

### 注意

> 版本“2.33”表示我想要使用确切的这个版本。我也可以只使用“2”来表示我可以使用“2.x”系列中的最新版本。还有许多其他方式来指定版本，我建议你阅读如何指定依赖项。

下次我尝试构建程序时，Cargo 将下载 `clap` 源代码（如果需要）以及它的所有依赖项。例如，我可以运行 `cargo build` 仅构建新的二进制文件而不运行它：

```
$ cargo build
    Updating crates.io index
Compiling libc v0.2.93
Compiling bitflags v1.2.1
Compiling unicode-width v0.1.8
Compiling vec_map v0.8.2
Compiling strsim v0.8.0
Compiling ansi_term v0.11.0
Compiling textwrap v0.11.0
Compiling atty v0.2.14
Compiling clap v2.33.3
Compiling echor v0.1.0 (/Users/kyclark/work/sysprog-rust/playground/echor)
    Finished dev [unoptimized + debuginfo] target(s) in 27.30s
```

你可能好奇这些包去了哪里。Cargo 将下载的源代码放在 `$HOME/.cargo` 中，而构建产物则放在 `target/debug/deps` 目录中。这引出了构建 Rust 项目的一个有趣方面：你构建的每个程序都可以使用不同版本的 crate，并且每个程序都在单独的目录中构建。如果你曾经因为使用 Perl 和 Python 中常见的共享模块而受苦，你会欣赏到你不必担心冲突的问题，即一个程序需要某个旧的冷门版本，而另一个程序需要 GitHub 上最新的前沿版本。当然，Python 提供了虚拟环境来解决这个问题，其他语言也有类似的解决方案。尽管如此，我发现 Rust 的方法相当令人安心。

Rust 将依赖项放在 target 目录中的一个后果是它现在相当大。我的项目目录已经接近 30MB，而几乎所有内容都位于 target 目录中。如果你运行 cargo help，你会看到 clean 命令会删除 target 目录，但代价是将来需要重新编译。如果你一段时间不会继续该项目，可以这样做来回收磁盘空间。

## 使用 clap 解析命令行参数

为了学习如何使用 clap 解析参数，我需要阅读文档。我喜欢使用 Docs.rs，“Rust 编程语言 crate 的开源文档托管服务”。我可以按照文档中的示例来创建新的 App 结构体。将你的 src/main.rs 改为如下内容：

```rust
use clap::App; ①

fn main() {
    let _matches = App::new("echor") ②
        .version("0.1.0") ③
        .author("Ken Youens-Clark <kyclark@gmail.com>") ④
        .about("Rust echo") ⑤
        .get_matches(); ⑥
}
```

导入 `clap::App` 结构体。

- 2. 使用名称 *echor* 创建一个新的 `App`。
- 3. 使用前面描述的语义版本号。
- 4. 你的姓名和电子邮件地址，以便人们知道把钱寄到哪里。
- 5. 这是程序的简短描述。
- 6. 要求 `App` 解析参数。

> **NOTE**
> 
> 在前面的代码中，变量名 `_matches` 中的前导下划线是有功能的。它告诉 Rust 编译器我现在不打算使用这个变量。如果没有下划线，编译器会警告未使用的变量。

有了这段代码，我就可以使用 `-h` 或 `--help` 标志运行程序以获取使用文档。请注意，我不需要定义此参数，因为 `clap` 已为我完成了：

```
$ cargo run -- -h
echor 0.1.0 ①
Ken Youens-Clark <kyclark@gmail.com> ②
Rust echo ③

USAGE:
    echor

FLAGS:
    -h, --help      Prints help information
    -V, --version   Prints version information
```

① 应用程序名称和 `version` 号码出现在这里。

② 这里是作者信息。

③ 这是关于文本。

除了帮助标志之外，我还看到 clap 也自动处理 -V 和 --version 标志以打印程序的版本：

```
$ cargo run -- --version
echor 0.1.0
```

接下来，我需要定义参数，我可以通过向 App 添加 Arg 结构体来实现。

```rust
use clap::{App, Arg}; ①

fn main() {
    let matches = App::new("echor")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust echo")
        .arg( ②
            Arg::with_name("text")
                .value_name("TEXT")
                .help("Input text")
                .required(true)
                .min_values(1),
        )
        .arg( ③
            Arg::with_name("omit_newline")
                .help("Do not print newline")
                .takes_value(false)
                .short("n"),
        )
        .get_matches(); ④

    println!("{{:#?}}", matches); ⑤
}
```

① 从 clap crate 导入 App 和 Arg 结构体。

- 2. 创建一个名称为 `text` 的新 `Arg`。这是一个必需的位置参数，必须至少出现一次并且可以重复。
- 3. 创建一个名称为 `omit_newline` 的新 `Arg`。这是一个只有短名称 `-n` 且不接受值的标志。
- 4. 解析参数并返回匹配的元素。
- 5. 漂亮地打印参数。

> **NOTE**
> 
> 之前我使用 `{ :? }` 来格式化参数的调试视图。这里我使用 `{ :#? }` 来包含换行和缩进以帮助我阅读输出。这被称为 *pretty-printing*，因为，嗯，它更漂亮。

如果你再次请求使用信息，你将看到新的参数：

```
$ cargo run -- --help
echor 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust echo

USAGE:
    echor [FLAGS] <TEXT>...

FLAGS:
    -h, --help          Prints help information
    -n                  Do not print newline ①
    -V, --version       Prints version information

ARGS:
    <TEXT>...           Input text ②
```

① 用于省略换行的 `-n` 标志是可选的。

② 必需的输入文本是一个或多个位置参数。

使用一些参数运行程序并检查参数的结构：

```
$ cargo run -- -n Hello world
ArgMatches {
    args: {
        "text": MatchedArg {
            occurs: 2,
            indices: [
                2,
                3,
            ],
            vals: [
                "Hello",
                "world",
            ],
        },
        "omit_newline": MatchedArg {
            occurs: 1,
            indices: [
                1,
            ],
            vals: [],
        },
    },
    subcommand: None,
    usage: Some(
        "USAGE:\n    echor [FLAGS] <TEXT>...",
    ),
}
```

如果你不带任何参数运行程序，你将收到一个错误，指出你未能提供必需的参数：

```
$ cargo run
error: The following required arguments were not provided:
    <TEXT>...

USAGE:
```



```
echor [FLAGS] <TEXT>...

For more information try --help
```

这是一个错误，因此你可以检查退出值以验证它不为 0：

```
$ echo $?
1
```

如果你尝试提供任何未定义的 argument，它会触发错误并产生非零退出值：

```
$ cargo run -- -x
error: Found argument '-x' which wasn't expected, or isn't valid in this context

USAGE:
    echor [FLAGS] <TEXT>...

For more information try --help
```

## 注意

你可能会想知道这种神奇的事情是如何发生的。为什么程序会停止并报告这些错误？如果你阅读 App::get_matches 的文档，你会看到“upon a failed parse an error will be displayed to the user and the process will exit with the appropriate error code。”

输出中还有另一件微妙且并不明显的事情。用法和错误信息都出现在 STDERR（读作 standard error）上，这是 Unix 输出的另一个通道。为了在 bash shell 中看到这一点，我可以将通道 1（STDOUT）重定向到名为 out 的文件，将通道 2（STDERR）重定向到名为 err 的文件。

```
$ cargo run 1>out 2>err
```

你应该看不到该命令的任何结果，因为所有输出都被重定向到了文件。out 文件应为空，因为没有内容打印到 STDOUT，但 err 文件应包含 Cargo 的输出和程序的错误信息：

```
$ cat err
    Finished dev [unoptimized + debuginfo] target(s) in
0.01s
    Running `target/debug/echor`
error: The following required arguments were not provided:
    <TEXT>...

USAGE:
    echor [FLAGS] <TEXT>...

For more information try --help
```

因此你可以看到，良好行为的系统程序的另一个标志是向 STDOUT 打印常规输出，向 STDERR 打印错误信息。有时错误严重到需要停止程序，但有时它们只需要在运行过程中记录下来。例如，在第 3 章中，你将编写一个处理输入文件的程序，其中一些文件故意不存在或无法读取。我将向你展示如何向 STDERR 打印这些文件的警告并跳到下一个参数。

## 创建程序输出

下一步是使用用户提供的值来创建程序的输出。常见的做法是将 matches 中的值复制到变量中。首先，我想提取 text 参数。因为这个 Arg 被定义为接受一个或多个值，所以我可以使用以下返回多个值的函数之一：

- ArgMatches::values_of: 返回 Option<Values>
- `ArgMatches::values_of_lossy`: 返回 `Option<Vec<String>>`

为了决定使用哪个，我需要深入了解以下几个概念：

- `Option`: 要么是 `None` 要么是 `Some(T)` 的值，其中 T 是任何类型，如字符串或整数。在 `ArgMatches::values_of_lossy` 的情况下，类型 T 将是一个字符串向量。
- `Values`: 用于从参数中获取多个值的迭代器。
- `Vec`: 一个向量，即连续的可增长数组类型。
- `String`: 字符的字符串。

两个函数 `ArgMatches::values_of` 和 `ArgMatches::values_of_lossy` 都会返回一个 `Option`。由于我最终想要打印字符串，我将使用 `ArgMatches::values_of_lossy` 函数获取一个 `Option<Vec<String>>`。`Option::unwrap` 函数会从 `Some(T)` 中取出值以获取负载 T。因为 text 参数由 `clap` 标记为必需，我知道不可能出现 `None`；因此，我可以安全地调用 `Option::unwrap` 来获取 `Vec<String>` 值：

```
let text = matches.values_of_lossy("text").unwrap();
```

## 警告

如果你对 `None` 调用 `Option::unwrap`，它会引发 `panic` 从而导致程序崩溃。你只有在确定该值是 `Some` 变体时才应调用 `unwrap`。

`omit_newline` 参数稍微简单一些，因为它要么存在要么不存在。此值的类型为 `bool` 或布尔值，为 `true` 或 `false`：

```
let omit_newline = matches.is_present("omit_newline");
```

最后，我想打印这些值。因为 `text` 是一个字符串向量，我可以使用 `Vec::join` 将所有字符串用单个空格连接成一个新字符串进行打印。在 `echor` 程序内部，`clap` 会创建该向量。为了演示 `Vec::join` 的工作原理，我将展示如何使用 `vec!` 宏创建向量：

```
let text = vec!["Hello", "world"];
```

## 注意

Rust 向量中的值必须全部为同一类型。动态语言通常允许列表混合类型，如字符串和数字，但 Rust 会抱怨“mismatched types。”这里我想要一个字面字符串的列表，这些字符串必须用双引号括起来。Rust 中的 `str` 类型表示有效的 UTF-8 字符串。我将在第 4 章中更多地谈论 UTF。

`Vec::join` 会在向量的所有元素之间插入给定的字符串以创建一个新字符串。我可以使用 `println!` 将新字符串打印到 `STDOUT` 后跟换行符：

```
println!("{} ", text.join(" "));
```

在 Rust 文档中，使用 `assert!` 来说明某事为 `true` 或使用 `assert_eq!` 来证明一件事等同于另一件事的做法很常见。在下面的代码中，我可以断言 `text.join(" ")` 的结果等于字符串 "Hello world"：

```
assert_eq!(text.join(" "), "Hello world");
```

当 -n 标志存在时，输出应省略换行符。我将改用 print! 宏，它不会添加换行符，并根据 omit_newline 的值选择添加换行符或空字符串。根据你的背景，你可能会尝试写类似这样的代码：

```
let ending = "\n"; ①
if omit_newline {
    ending = ""; // This will not work ②
}
print!("{}{}", text.join(" "), ending); ③
```

- ① 分配默认值。
- ② 如果应省略换行符则更改值。
- ③ 使用 print!，它不会在输出中添加换行符。

如果我尝试运行这段代码，Rust 会告诉我不能重新分配 ending 的值：

```
$ cargo run -- Hello world
error[E0384]: cannot assign twice to immutable variable `ending`
  --> src/main.rs:27:9
  |
25 |     let ending = "\n";
  |           ------
  |           |
  |           first assignment to `ending`
  |           help: make this binding mutable: `mut ending`
26 |     if omit_newline {
27 |         ending = ""; // This will not work
  |         ^^^^^^^^^^^ cannot assign twice to immutable variable
```

真正使 Rust 与其他语言区别开来的一个特点是变量默认是 *immutable*，意味着它们不能从初始值更改。我不允许重新分配变量 ending 的值；然而，编译器告诉我添加 mut 使变量变为 *mutable*：

```
let mut ending = "\n";
if omit_newline {
    ending = "";
}
print!("{}{}", text.join(" "), ending);
```

① 添加 mut 使其成为可变值。

有一种更好的写法。在 Rust 中，if 是一个表达式而不是语句。*expression* 会返回一个值，但 statement 不会。这里是一种更地道的写法：

```
let ending = if omit_newline { "" } else { "\n" };
```

## 注意

> 没有 else 的 if 会返回 unit 类型。函数没有返回类型时也是如此，因此 main 函数返回 ()。

由于我只在一个地方使用 ending，因此不需要将其赋值为变量。以下是更新 main 函数的方式：

```
fn main() {
    let matches = ...; // Same as before
    let text = matches.values_of_lossy("text").unwrap();
    let omit_newline = matches.is_present("omit_newline");
    print!("{}{}", text.join(" "), if omit_newline { "" } else { "\n" });
}
```

经过这些更改，程序似乎工作正常；然而，我不愿把名声押在这上面。我需要如俄语谚语所说，“Доверяй, но проверяй。”¹ 这要求我编写一些测试，用各种输入运行程序并验证它是否产生与原始 echo 程序相同的输出。

## 集成和单元测试

在第 1 章中，我展示了如何创建集成测试，这些测试从命令行运行程序，就像用户会做的那样，以确保其正确工作。在本章中，我还将向你展示如何编写单元测试来测试单个函数，这可以被视为编程的单元。开始时，你应该向 Cargo.toml 添加以下依赖项。请注意，我正在向此项目添加 predicates：

```toml
[dev-dependencies]
assert_cmd = "1"
predicates = "1"
```

我经常编写测试来确保程序在错误运行时失败。例如，当未提供参数时，此程序应失败并打印帮助文档。创建一个 tests 目录，然后创建 tests/cli.rs 并写入以下内容：

```rust
use assert_cmd::Command;
use predicates::prelude::*; ①

#[test]
fn dies_no_args() {
    let mut cmd = Command::cargo_bin("echor").unwrap();
    cmd.assert() ②
        .failure()
        .stderr(predicate::str::contains("USAGE"));
}
```

导入 `predicates` crate。

2. 使用无参数运行程序并断言其失败并向 `STDERR` 打印使用说明。

> **注意**
> 
> 我通常为这类测试命名时使用前缀 *dies*，这样我就可以使用 `cargo test dies` 运行它们所有，以确保程序在各种条件下失败。

我还可以添加一个测试以确保在提供参数时程序成功退出：

```
#[test]
fn runs() {
    let mut cmd = Command::cargo_bin("echor").unwrap();
    cmd.arg("hello").assert().success(); ①
}
```

1. 使用参数“hello”运行 `echor` 并验证其成功退出。

## 创建测试输出文件

我现在可以运行 `cargo test` 来验证我有一个可以运行、验证用户输入并打印使用的程序。接下来，我想确保程序创建的输出与 `echo` 相同。为了开始，我需要捕获原始 `echo` 对各种输入的输出，以便将其与我的程序的输出进行比较。在我的 GitHub 仓库的 *02_echor* 目录中，你会找到一个名为 *mk-outs.sh* 的 `bash` 脚本，我用它为各种参数生成 `echo` 的输出。你可以看到，即使对于这样一个简单的工具，仍然存在相当多的 *圈复杂度*，它指的是所有参数可以组合的各种方式。我需要检查一个或多个文本参数在有和没有换行选项的情况下：

```bash
$ cat mk-outs.sh
#!/usr/bin/env bash ①

OUTDIR="tests/expected" ②
[[ ! -d "$OUTDIR" ]] && mkdir -p "$OUTDIR" ③

echo "Hello there" > $OUTDIR/hello1.txt ④
echo "Hello"  "there" > $OUTDIR/hello2.txt ⑤
echo -n "Hello  there" > $OUTDIR/hello1.n.txt ⑥
echo -n "Hello"  "there" > $OUTDIR/hello2.n.txt ⑦
```

- ① “shebang” 行告诉操作系统使用环境为以下代码执行 `bash`。
- ② 为输出目录定义一个变量。
- ③ 测试输出目录是否存在，如果不存在则创建它。
- ④ 一个包含两个词的参数。
- ⑤ 用多个空格分隔的两个参数。
- ⑥ 一个包含两个空格且没有换行的参数。
- ⑦ 两个参数且没有换行。

如果你在 Unix 平台上工作，你可以将此程序复制到项目目录并像这样运行它：

```bash
$ bash mk-outs.sh
```

也可以直接执行该程序，但如果你收到 *permission denied* 错误，可能需要执行 `chmod +x mk-outs.sh`：

```
$ ./mk-outs.sh
```

如果成功，你现在应该有一个 *tests/expected* 目录，其内容如下：

```
$ tree tests
tests
├── cli.rs
└── expected
    ├── hello1.n.txt
    ├── hello1.txt
    ├── hello2.n.txt
    └── hello2.txt

1 directory, 5 files
```

如果你在 Windows 平台上工作，我建议你将此目录结构复制到你的项目中。现在你应该有一些测试文件可用于比较程序的输出。

## 比较程序输出

第一个输出文件是用输入 *Hello there* 作为单个字符串生成的，并且输出被捕获到文件 *tests/expected/hello1.txt* 中。对于我的下一个测试，我将使用此参数运行 `echor` 并将输出与该文件的内容进行比较。请确保在 *tests/cli.rs* 中添加 `use std::fs` 以引入标准文件系统模块，然后用以下内容替换 `runs` 函数：

```
#[test]
fn hello1() {
    let outfile = "tests/expected/hello1.txt"; ①
    let expected = fs::read_to_string(outfile).unwrap(); ②
    let mut cmd = Command::cargo_bin("echor").unwrap(); ③
    cmd.arg("Hello
there").assert().success().stdout(expected); ④
}
```

- 1. 这是 `mk-outs.sh` 生成的 `echo` 的输出。
- 2. 使用 `fs::read_to_string` 读取文件的内容。这会返回一个 `Result`，如果一切顺利则可能包含一个字符串。使用 `Result::unwrap` 方法并假设它会成功。
- 3. 创建一个 `Command` 在当前 crate 中运行 `echor`。
- 4. 使用给定的参数运行程序并断言其成功完成且 `STDOUT` 为预期值。

> **警告**
> 
> `fs::read_to_string` 是将文件读入内存的便捷方法，但如果读取的文件超过可用内存，它也是导致程序崩溃——甚至可能导致你的计算机崩溃——的简便方法。你应该只用此函数读取小文件。正如 Ted Nelson 所说，“计算机的好处在于它们按你告诉它们的方式做事。坏处在于它们按你告诉它们的方式做事。”

如果你运行 `cargo test`，你可能会看到如下输出：

```
running 2 tests
test hello1 ... ok
test dies_no_args ... ok
```

## 使用 Result 类型

我一直在使用 `Result::unwrap` 方法，以假设每个可能失败的调用都会成功。例如，在 `hello1` 函数中，我假设输出文件存在并且可以打开并读入字符串。在有限的测试中，这可能是正确的，但做出此类假设很危险。我宁愿更谨慎一些，所以我要创建一个名为 `TestResult` 的类型别名。

这将是一个特定的 `Result` 类型，它要么是始终包含单元类型的 `Ok`，要么是实现 `std::error::Error` trait 的某个值：

```
type TestResult = Result<(), Box<dyn std::error::Error>>;
```

在前面的代码中，`Box` 表示错误将存放在一种指针内部，其中内存是在堆上动态分配而不是在栈上，并且 `dyn` “用于突出显示对关联 Trait 方法的调用是动态分派的。” 这确实是很多信息，我不怪你如果你的眼睛已经模糊了。简而言之，我说的是 `TestResult` 的 `Ok` 部分将永远只包含单元类型，而 `Err` 部分可以包含实现 `std::error::Error` trait 的任何内容。这些概念在 *Programming Rust*（O'Reilly，2021）中得到了更彻底的解释。

## 栈和堆内存

在学习 Rust 编程之前，我只考虑过一种模糊的计算机内存概念。刻意避免需要分配和释放内存的语言，我只模糊地意识到动态语言为我隐藏这些复杂性的努力。在 Rust 中，我了解到并非所有内存都以相同的方式访问。首先，有 *栈*，其中已知大小的项目以特定顺序访问。经典的类比是餐盘堆，干净的盘子放在上面，并在 *后进先出*（LIFO）顺序中从顶部取回。栈上的项目具有固定的已知大小，使得 Rust 能够为其预留特定的内存块并快速找到它。

另一种内存类型是 *堆*，其中值的大小可能随时间变化。例如，vector 的文档将此结构描述为“连续的可增长数组类型”。*可增长的* 是关键词，因为 vector 中元素的数量和大小可能在程序生命周期中发生变化。Rust 会估计 vector 所需的内存量。如果 vector 超出原始分配，Rust 将找到另一块内存来保存数据。为了找到数据所在的内存，Rust 将内存地址存储在栈上。这称为 *指针*，因为它指向实际数据，因此也被称为对数据的 *引用*。Rust 知道如何 *解引用* 一个 Box 以找到数据。

这以一些微妙的方式改变了我的测试代码。所有函数现在都表示返回 TestResult。以前我使用 Result::unwrap 来解包 Ok 值，并在出现 Err 时 panic，导致测试失败。在以下代码中，我用 `?` 运算符替换 unwrap，以解包 Ok 值或将 Err 值传播到返回类型。也就是说，这将导致函数向调用者返回 Option 的 Err 变体，这反过来将导致测试失败。如果测试函数中的所有代码都成功运行，我返回包含单元类型的 Ok 以表示测试通过。请注意，虽然 Rust 确实有 return 来提前从函数返回一个值，但惯例是省略最后一个表达式的分号以隐式返回该结果。将你的 tests/cli.rs 更新为以下内容：

```
use assert_cmd::Command;
use predicates::prelude::*;
use std::fs;

type TestResult = Result<(), Box<dyn std::error::Error>>;

#[test]
fn dies_no_args() -> TestResult {
    let mut cmd = Command::cargo_bin("echor")?;
    cmd.assert()
        .failure()
        .stderr(predicate::str::contains("USAGE"));
    Ok(())
}

#[test]
fn hello1() -> TestResult {
    let expected =
    fs::read_to_string("tests/expected/hello1.txt")?;
    let mut cmd = Command::cargo_bin("echor")?;
    cmd.arg("Hello
there").assert().success().stdout(expected);
    Ok(())
}
```

- ① 使用 ? 代替 Result::unwrap 来解包 Ok 值或传播 Err。
- ② 省略最后的分号以返回此值。




下一个测试传递两个参数 Hello 和 there，并期望程序打印 Hello there。

```
#[test]
fn hello2() -> TestResult {
    let expected =
fs::read_to_string("tests/expected/hello2.txt")?;
    let mut cmd = Command::cargo_bin("echor")?;
    cmd.args(vec!["Hello", "there"]) ①
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

① 使用 Command::args 方法传递参数向量，而不是单个字符串值。

我总共有四个文件需要检查，因此最好为此编写一个辅助函数。我将它命名为 run，并将参数字符串以及期望的输出文件传递给它。与其为参数使用向量，我将使用 std::slice，因为我在定义参数列表后不需要增长它：

```
fn run(args: &[&str], expected_file: &str) -> TestResult {
    let expected = fs::read_to_string(expected_file)?; ②
    Command::cargo_bin("echor")? ③
        .args(args)
        .assert()
        .success()
        .stdout(expected);
    Ok(()) ④
}
```

① args 将是一个 &str 值的切片，expected_file 将是一个 &str。返回值为 TestResult。

- 2. 尝试将 `expected_file` 的内容读入一个字符串。
- 3. 尝试在当前 crate 中使用给定参数运行 `echor` 并断言 `STDOUT` 为预期值。
- 4. 如果之前的所有代码都正常工作，则返回包含单元类型的 `Ok`。

## NOTE

你会发现 Rust 有许多种“字符串”变量类型。这里 `str` 类型适用于源代码中的字面字符串。& 表示我只打算借用字符串一小段时间。我稍后会更多地谈论字符串、借用和所有权。

下面是如何使用辅助函数运行全部四个测试。请用以下内容替换之前的 `hello1` 和 `hello2` 定义：

```
#[test]
fn hello1() -> TestResult {
    run(&["Hello there"], "tests/expected/hello1.txt") ①
}

#[test]
fn hello2() -> TestResult {
    run(&["Hello", "there"], "tests/expected/hello2.txt")
②
}

#[test]
fn hello1_no_newline() -> TestResult {
    run(&["Hello there", "-n"],
"tests/expected/hello1.n.txt") ③
}

#[test]
fn hello2_no_newline() -> TestResult {
    run(&["-n", "Hello", "there"],
"tests/expected/hello2.n.txt") ④
}
```

- 1. 输入为单个字符串值。
- 2. 输入为两个字符串。
- 3. 输入为单个字符串值，并带 `-n` 标志以省略换行。注意单词之间有两个空格。
- 4. 输入为两个字符串，`-n` 标志出现在最前面。

正如你所见，我可以在 *tests/cli.rs* 中编写任意多的函数。只有标记为 `#[test]` 的函数在测试时才会运行。如果现在运行 **cargo test**，你应该会看到五个通过的测试（顺序不固定）：

```
running 5 tests
test dies_no_args ... ok
test hello1 ... ok
test hello1_no_newline ... ok
test hello2_no_newline ... ok
test hello2 ... ok
```

## Summary

现在你已经在 *src/main.rs* 中为 `echor` 程序编写了大约 30 行 Rust 代码，并在 *tests/cli.rs* 中编写了五个测试，以验证你的程序是否满足某种程度的规范（他们所说的 *specs*）。考虑一下你所取得的成就：

- 良好的系统程序应将基本输出打印到 `STDOUT`，错误打印到 `STDERR`。
- 你编写了一个程序，它接受选项 `-h|--help` 以生成帮助，`-V|--version` 以显示程序版本，以及 `-n` 以省略换行，同时接受一个或多个位置命令行参数。
- 如果程序使用错误的参数运行或使用 `-h|--help` 标志运行，它将打印使用文档。
- 程序会将所有命令行参数用空格连接后回显。
- 如果存在 `-n` 标志，则尾随换行将被省略。
- 你可以运行集成测试，以确认你的程序在至少四个测试用例中复制了 `echo` 的输出，涵盖一到两个输入，带或不带尾随换行。
- 你学会了使用多种 Rust 类型，包括单元类型、字符串、向量、切片、`Option` 和 `Result`，以及如何为特定类型的 `Result` 创建类型别名，称为 `TestResult`。
- 你使用 `Box` 创建了指向堆内存的智能指针。这需要深入了解栈——变量具有固定已知大小且按 LIFO 顺序访问——与堆之间的差异——变量大小可能在程序执行期间发生变化，并通过指针访问。
- 你学会了如何将文件的全部内容读入字符串。
- 你学会了如何从 Rust 程序内部执行外部命令，检查退出状态，并验证 `STDOUT` 和 `STDERR` 的内容。

所有这些，而你是在一种根本不允许你犯导致有缺陷程序或安全漏洞的常见错误的语言中编写的。尽情给自己一个小小的击掌，或在思考 Rust 如何帮助你征服世界时享受一下略带邪恶的 MWUHAHA 笑声。现在我已经展示了如何组织和编写测试和数据，我将在下一个程序中再次使用之前的测试，以便我可以开始使用 *test-driven development*，即先编写测试然后编写代码以满足测试。

1 “Trust, but verify.” 据说这在俄语中押韵，所以听起来比里根在 20 世纪 80 年代与苏联进行核裁军谈判时使用的更酷。
