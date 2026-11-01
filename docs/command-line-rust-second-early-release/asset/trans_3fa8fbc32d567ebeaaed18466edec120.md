
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
