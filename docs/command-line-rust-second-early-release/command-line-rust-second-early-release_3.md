# Chapter 3. On The Catwalk

### A NOTE FOR EARLY RELEASE READERS

使用 Early Release 电子书，你可以在这些标题的正式发布之前很久就利用这些技术——作者在写作时提供的原始未编辑内容。

这将是最终书籍的第 3 章。请注意 GitHub 仓库稍后才会激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者在本章中注意到缺失的材料，请联系作者 kyclark@gmail.com。

> *当你独自一人时，你是猫，你是电话。你是一只动物。*
> 
> —They Might Be Giants

在本章中，挑战是编写 `cat` 程序的克隆，该程序之所以得名是因为它可以将 *concatenate* 多个文件到一个文件中。也就是说，给定文件 *a*、*b* 和 *c*，你可以执行 `cat a b c > all` 来将这三个文件的所有行流式传输并重定向到名为 *all* 的文件。该程序将接受一个选项来为每一行加上行号前缀。

在本章中，你将学习：

- 如何将代码组织成库和二进制 crate
- 如何使用测试先行开发
- 公共变量和函数与私有变量和函数之间的区别
- 如何测试文件的存在
- 如何为不存在的文件创建随机字符串
- 如何读取常规文件或 `STDIN`（发音为 *standard in*）
- 如何使用 `eprintln!` 打印到 `STDERR` 以及使用 `format!` 格式化字符串
- 如何编写提供 `STDIN` 输入的测试
- 如何以及为何创建 `struct`
- 如何定义互斥参数
- 如何使用迭代器的 `enumerate` 方法
- 关于如何以及为何使用 `Box` 的更多内容

## How cat Works

我将首先展示 cat 的工作原理，以便你了解挑战的期望。BSD 版本的 cat 不响应 --help，因此我必须使用 man cat 来阅读手册页。对于这样一个有限的程序，它有惊人的多个选项：

```
CAT(1)                              BSD General Commands Manual
CAT(1)

NAME
    cat -- concatenate and print files

SYNOPSIS
    cat [-benstuv] [file ...]

DESCRIPTION
```

cat 实用程序按顺序读取文件，将它们写入标准输出。文件操作数按命令行顺序处理。如果 file 是单个连字符 ('-') 或缺失，cat 将从标准输入读取。如果 file 是 UNIX 域套接字，cat 将连接到它然后一直读取到 EOF。这补充了 inetd(8) 中可用的 UNIX 域绑定功能。

选项如下：

- **-b** 对非空白输出行编号，从 1 开始。
- **-e** 显示不可打印字符（参见 -v 选项），并在每一行末尾显示美元符号 ('$')。
- **-n** 对输出行编号，从 1 开始。
- **-s** 压缩多个相邻空行，使输出单倍行距。
- **-t** 显示不可打印字符（参见 -v 选项），并将制表符显示为 '^I'。
- **-u** 禁用输出缓冲。
- **-v** 显示不可打印字符使其可见。控制字符打印为 '^X' 表示 control-X；删除字符（八进制 0177）打印为 '^*?'。带高位设置的非 ASCII 字符打印为 'M-'（表示 meta）后跟低 7 位的字符。

## EXIT STATUS



cat 工具在成功时退出码为 0，发生错误时退出码 >0。

## GNU 版本确实响应 --help：

```
$ cat --help
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s), or standard input, to standard output.

  -A, --show-all          equivalent to -vET
  -b, --number-nonblank   number nonempty output lines,
overrides -n
  -e                      equivalent to -vE
  -E, --show-ends         display $ at end of each line
  -n, --number            number all output lines
  -s, --squeeze-blank     suppress repeated empty output
lines
  -t                      equivalent to -vT
  -T, --show-tabs         display TAB characters as ^I
  -u                      (ignored)
  -v, --show-nonprinting  use ^ and M- notation, except
for LFD and TAB
      --help    display this help and exit
      --version output version information and exit
```

如果没有 FILE，或者 FILE 为 -，则读取标准输入。

示例：

cat f - g  Output f's contents, then standard input, then g's contents.
cat        Copy standard input to standard output.

GNU coreutils 在线帮助：
<http://www.gnu.org/software/coreutils/>
完整文档请运行：info coreutils 'cat invocation'

> NOTE
> 
> BSD 版本早于 GNU 版本，因此后者实现了所有相同的短标志以保持兼容性。如 GNU 程序的典型做法，它还提供了长标志别名，如 `-n` 的 `--number` 和 `-b` 的 `--number-nonblank`。我将向你展示如何像 GNU 版本一样提供这两种选项。

对于挑战程序，我将只实现选项 `-b|--number-nonblank` 和 `-n|--number`。我还将展示如何在文件名参数为 "-" 时读取常规文件和 `STDIN`。我已将四个测试文件放入 `03_catr/tests/inputs` 目录中：

- 1. *empty.txt*：一个空文件
- 2. *fox.txt*：一行文本
- 3. *spiders.txt*：三行文本
- 4. *the-bustle.txt*：艾米莉·狄金森的一首 lovely 诗，共九行包括一行空白

空文件很常见，虽然没什么用。我包含它是为了确保我的程序能够优雅地处理意外输入。也就是说，我希望我的程序至少不会崩溃。以下命令不产生输出，因此我期望我的程序也一样：

```
$ cd 03_catr
$ cat tests/inputs/empty.txt
```

接下来，我将在一个包含一行文本的文件上运行 `cat`：

```
$ cat tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

`-n|--number` 和 `-b|--number-nonblank` 标志都会对行进行编号，且行号在宽为六个字符的字段中右对齐，后跟一个制表符，然后是文本行。

为了区分制表符，我可以使用 -t 选项显示非打印字符，这样制表符会显示为 ^I。在以下命令中，我使用 Unix 管道 | 将第一个命令的 STDOUT 连接到第二个命令的 STDIN：

```
$ cat -n tests/inputs/fox.txt | cat -t
    1^IThe quick brown fox jumps over the lazy dog.
```

spiders.txt 文件有三行文本，应使用 -b 选项编号：

```
$ cat -b tests/inputs/spiders.txt
    1  Don't worry, spiders,
    2  I keep house
    3  casually.
```

-n（左侧）和 -b（右侧）之间的区别仅在 the-bustle.txt 上明显，因为后者只对非空白行编号：

```
$ cat -n tests/inputs/the-bustle.txt      $ cat -b tests/inputs/the-bustle.txt
    1  The bustle in a house                1  The bustle in a house
    2  The morning after death              2  The morning after death
    3  Is solemnest of industries           3  Is solemnest of industries
    4  Enacted upon earth,−                 4  Enacted upon earth,−
    5
    6  The sweeping up the heart,           5  The sweeping up the heart,
    7  And putting love away                6  And putting love away
    8  We shall not want to use again       7  We shall not want to use again
    9  Until eternity.                      8  Until eternity.
```

## NOTE

奇怪的是，你可以同时使用 -b 和 -n，且 -b 选项具有优先权。挑战程序将只允许其中一个。

当处理任何不存在或无法打开的文件时，cat 会向 STDERR 打印一条消息并继续处理下一个文件。在以下示例中，我使用 blargh 作为不存在的文件。我使用 touch 命令创建文件 cant-touch-this，并使用 chmod 命令将文件权限设置为使其无法读取。你将在第 15 章编写 ls 克隆时了解更多关于 000 的含义：

```
$ touch cant-touch-this && chmod 000 cant-touch-this
$ cat tests/inputs/fox.txt blargh tests/inputs/spiders.txt cant-touch-this
The quick brown fox jumps over the lazy dog. ①
cat: blargh: No such file or directory ②
Don't worry, spiders, ③
I keep house
casually.
cat: cant-touch-this: Permission denied ④
```

- ① 这是第一个文件的输出。
- ② 这是针对不存在文件的错误。
- ③ 这是第三个文件的输出。
- ④ 这是针对无法读取文件的错误。

最后，使用所有文件运行 cat，注意它会为每个文件重新编号行：

```
$ cd tests/inputs
$ cat -n empty.txt fox.txt spiders.txt the-bustle.txt
```

```
1  The quick brown fox jumps over the lazy dog.
1  Don't worry, spiders,
2  I keep house
3  casually.
1  The bustle in a house
2  The morning after death
3  Is solemnest of industries
4  Enacted upon earth,–
5
6  The sweeping up the heart,
7  And putting love away
8  We shall not want to use again
9  Until eternity.
```

如果你查看 *mk-outs.sh* 脚本，你会看到我使用 `cat` 以所有这些文件单独和一起的方式作为常规文件和通过 `STDIN`，使用无标志以及 `-n` 和 `-b` 标志。我将所有输出捕获到 *tests/expected* 目录中的各种文件中以用于测试。

## 开始使用测试驱动开发

在第 2 章中，我在章节末尾编写了测试，因为我需要向你展示语言的一些基础知识。从这个练习开始，我希望你思考 *test-driven development*（TDD），如 Kent Beck 所著的同名书籍（Addison-Wesley，2002）所述。TDD 提倡在编写代码 *之前* 编写代码的测试，如图 3-1 所示。

- 添加一个测试
- 运行所有测试并看到新测试失败
- 编写代码
- 运行测试
- 重构代码
- 重复

*图 3-1. 测试驱动开发循环*

## NOTE

从技术上讲，TDD 涉及在添加功能时编写每个测试。由于我已为你编写了所有测试，你可能认为这更像是 *test-first development*。一旦你编写了通过测试的代码，你就可以开始重构代码以改进它，也许通过缩短代码行或找到更快的实现。

你编写的挑战程序应命名为 `catr`（发音为 *cat-er*），即 `cat` 的 Rust 版本。我建议你使用 `cargo new catr` 开始一个新应用程序，然后将我的 *03_catr/tests* 目录复制到你的源树中。不要复制除测试之外的任何内容，因为你将自己编写其余代码。你应该具有如下结构：

```
$ tree -L 2 catr/
catr
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── cli.rs
    ├── expected
    └── inputs

4 directories, 3 files
```

我将使用第 2 章中的所有相同外部 crate，外加用于测试的 `rand` crate，因此将你的 *Cargo.toml* 更新为以下内容：

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

现在运行 `cargo test` 以下载 crate、编译你的程序并运行测试。所有测试都应该失败。你的任务，如果你选择接受，就是编写一个能够通过这些测试的程序。

## 创建库 crate

第 2 章中的程序相当简短，可以轻松放入 *src/main.rs*。你在职业生涯中编写的典型程序可能要长得多。从这个程序开始，我将把代码分为 *src/lib.rs* 中的库和调用库函数的 *src/main.rs* 中的二进制文件。我认为这种组织方式使得随着时间的推移更容易测试和扩展应用程序。

首先，我将把 *src/main.rs* 中的所有重要部分移动到 *src/lib.rs* 中的一个名为 `run` 的函数中。此函数将返回一种 `Result` 以指示成功或失败。这与第 2 章中的 `TestResult` 类型别名类似。`TestResult` 总是在 `Ok` 变体中返回单元类型 `()`，但 `MyResult` 可以返回包含任何类型的 `Ok`，我可以使用泛型 `T` 表示：

```
use std::error::Error; ①

type MyResult<T> = Result<T, Box<dyn Error>>;②

pub fn run() -> MyResult<()> { ③
    println!("Hello, world!"); ④
    Ok(()) ⑤
}
```

- ① 导入 `Error` trait 以表示错误值。
- ② 创建 `MyResult` 以表示任何类型 `T` 的 `Ok` 值或实现 `Error` trait 的某些 `Err` 值。



+   3. 定义一个 *公共* (`pub`) 函数，该函数返回包含单元类型 `()` 的 `Ok` 或某个错误 `Err`。
+   4. 打印 *你好，世界！*。
+   5. 返回一个表示函数运行成功的指示。

## TIP

默认情况下，模块中的所有变量和函数都是私有的。在前面的代码中，我必须使用 `pub` 使此库函数对程序的其余部分可访问。

调用此函数，将 `src/main.rs` 改为如下：

```rust
fn main() {
    if let Err(e) = catr::run() { ①
        eprintln!("{}", e); ②
        std::process::exit(1); ③
    }
}
```

- ① 执行 `catr::run` 函数并检查返回值是否匹配 `Err(e)`，其中 `e` 是实现 `Error` trait 的某个值，这意味着它可以被打印等。
- ② 使用 `eprintln!` (*错误打印行*) 宏将错误消息 `e` 打印到 `STDERR`。
- ③ 使用非零值退出程序以指示错误。

> TIP
> 
> `eprint!` 和 `eprintln!` 宏与 `print!` 和 `println!` 完全一样，只是它们打印到 STDERR。

如果执行 **cargo run**，你应该像之前一样看到 *你好，世界！*。

## 定义参数

接下来，我将添加程序的参数，并想引入一个名为 `Config` 的 `struct` 来表示程序的参数。`struct` 是一种数据结构，你可以在其中定义它将包含的元素名称和类型。它类似于其他语言中的类定义。在本例中，我想要一个 `struct` 来描述程序所需的数值，例如输入文件名的列表和用于编号输出行的标志。

将以下 `struct` 添加到 `src/lib.rs`。通常将此类定义放在 `use` 语句之后靠近顶部的位置：

```
#[derive(Debug)] ①
pub struct Config { ②
    files: Vec<String>, ③
    number_lines: bool, ④
    number_nonblank_lines: bool, ⑤
}
```

- ① `derive` 宏允许我添加 `Debug` trait，以便 struct 可以被打印。
- ② 定义一个名为 `Config` 的 `struct`。`pub`（公共）使其在库外部可访问。
- ③ `files` 将是一个字符串向量。
- ④ 这是一个布尔值，用于指示是否打印行号。
- ⑤ 这是一个布尔值，用于控制仅对非空白行打印行号。

要使用 struct，我可以用特定值创建它的实例。在下面的 get_args 函数草图中，你可以看到它最终使用来自用户的运行时值创建一个新的 Config。向你的 src/lib.rs 添加 use clap:: {App, Arg} 和此函数。尝试自行完成该函数，可以从第 2 章借鉴你能借鉴的内容：

```rust
pub fn get_args() -> MyResult<Config> { ①
    let matches = App::new("catr")... ②

    Ok(Config { ③
        files: ...,
        number_lines: ...,
        number_nonblank_lines: ...,
    })
}
```

- ① 这是一个公共函数，返回一个 MyResult，在成功时包含 Config 或错误。
- ② 在这里你应该定义参数并处理匹配项。
- ③ 使用提供的数值创建一个 Config。

这意味着 run 函数需要更新以接受 Config 参数。现在，先打印它：

```rust
pub fn run(config: Config) -> MyResult<()> { ①
    dbg!(config); ②
    Ok(())
}
```

- ① 该函数将接受一个 Config struct，如果成功则返回包含单元类型的 Ok。
- ② 使用 dbg!（调试）宏打印配置。

按如下方式更新你的 src/main.rs：

```rust
fn main() {
    if let Err(e) = catr::get_args().and_then(catr::run) {
        eprintln!("{}", e); ②
        std::process::exit(1); ③
    }
}
```

- ① 调用 catr::get_args 函数，然后使用 Result::and_then 将 Ok(config) 传递给 catr::run。
- ② 如果 get_args 或 run 返回 Err，则将其打印到 STDERR。
- ③ 使用非零值退出程序。

看看你能否让程序打印如下的用法：

```
$ cargo run --quiet -- --help
catr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust cat

USAGE:
    catr [FLAGS] <FILE>...

FLAGS:
    -h, --help            Prints help information
    -n, --number          Number lines
    -b, --number-nonblank Number non-blank lines
    -V, --version         Prints version information
```

参数：
    <FILE>...    输入文件 [默认值: -]

如果没有参数，你的程序应该能够打印如下的配置结构：

```
$ cargo run
[src/lib.rs:52] config = Config {
    files: [ ①
        "-",
    ],
    number_lines: false, ②
    number_nonblank_lines: false,
}
```

- ① 默认文件应包含用于 STDIN 的 “-”。
- ② 布尔值应默认为 false。

使用参数运行并确保配置如下所示：

```
$ cargo run -- -n tests/inputs/fox.txt
[src/lib.rs:52] config = Config {
    files: [
        "tests/inputs/fox.txt", ①
    ],
    number_lines: true, ②
    number_nonblank_lines: false,
}
```

- ① 位置文件参数被解析到 files 中。
- ② -n 选项使 number_lines 为 true。

虽然 BSD 版本允许同时使用 -n 和 -b，但挑战程序应将它们视为互斥，并在同时使用时生成错误：

```
$ cargo run -- -b -n tests/inputs/fox.txt
error: The argument '--number-nonblank' cannot be used with '--number'
'--number'
```

试试看。说真的！我想让你在阅读后续内容之前先尝试编写自己的版本。我会在这里等你完成。

准备好了吗？将你拥有的内容与我的 get_args 函数进行比较：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("catr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust cat")
        .arg(
            Arg::with_name("files") ①
                .value_name("FILE")
                .help("Input file(s)")
                .required(true)
                .default_value("-")
                .min_values(1),
        )
        .arg(
            Arg::with_name("number") ②
                .help("Number lines")
                .short("n")
                .long("number")
                .takes_value(false)
                .conflicts_with("number_nonblank"),
        )
        .arg(
            Arg::with_name("number_nonblank") ③
                .help("Number non-blank lines")
                .short("b")
                .long("number-nonblank")
                .takes_value(false),
        )
        .get_matches();

    Ok(Config {
        files: matches.values_of_lossy("files").unwrap(),

        number_lines: matches.is_present("number"), ⑤
        number_nonblank_lines:
```

```
matches.is_present("number_nonblank"),
    })
}
```

- 1. 这个位置参数用于文件，并且要求至少有一个值，默认为 "-".
- 2. 这是一个选项，具有短名称 `-n` 和长名称 `--number`。它不接受值，因为它是一个标志。当存在时，它会告诉程序打印行号。它不能与 `-b` 一起出现。
- 3. `-b|--number-nonblank` 标志控制是否对非空白行打印行号。
- 4. 因为至少需要一个值，所以调用 `Option::unwrap` 应该是安全的。
- 5. 这两个布尔选项要么存在要么不存在。

TIP

可选参数具有短名称和/或长名称，但位置参数没有。你可以在位置参数之前或之后定义可选参数。使用 `min_values` 定义位置参数也意味着多个值，但对可选参数不适用。

有了这么多代码，当你执行 `cargo test` 时，你应该能够通过至少几个测试。会有大量输出显示所有失败的测试输出，但不要气馁。你很快就会看到完全通过的测试套件。

## 处理文件

现在你已经验证了所有参数，可以处理文件并创建正确的输出。首先修改 src/lib.rs 中的 run 函数以打印每个文件名：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files { ①
        println!("{}", filename); ②
    }
    Ok(())
}
```

- ① 遍历每个文件名。
- ② 打印文件名。

使用一些输入文件运行程序。在以下示例中，bash shell 将文件 glob *.txt 扩展为以 .txt 扩展名结尾的所有文件名：

```
$ cargo run -- tests/inputs/*.txt ①
tests/inputs/empty.txt
tests/inputs/fox.txt
tests/inputs/spiders.txt
tests/inputs/the-bustle.txt
```

- ① TK

Windows PowerShell 可以使用 Get-ChildItem 扩展文件 glob：

```
> cargo run -q -- -n (Get-ChildItem .\tests\inputs\*.txt)
C:\Users\kyclark\work\rust-
sysprog\03_catr\tests\inputs\empty.txt
C:\Users\kyclark\work\rust-
sysprog\03_catr\tests\inputs\fox.txt
C:\Users\kyclark\work\rust-
sysprog\03_catr\tests\inputs\spiders.txt
C:\Users\kyclark\work\rust-
sysprog\03_catr\tests\inputs\the-bustle.txt
```



## 打开文件或 STDIN

下一步是尝试打开每个文件名。当文件名是“-”时，我应打开 STDIN；否则，我将尝试打开给定的文件名并处理错误。对于以下代码，你需要将导入扩展为如下所示：

```rust
use clap::{App, Arg};
use std::error::Error;
use std::fs::File;
use std::io::{self, BufRead, BufReader};
```

这一步有点棘手，所以我想为你提供一个 open 函数供你使用。在以下代码中，我使用了 match 关键字，它类似于 C 语言中的 switch 语句。具体来说，我是在匹配 filename 是否等于“-”或其它内容，后者使用通配符 _ 指定：

```rust
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> { ①
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        ②
        _ =>
        Ok(Box::new(BufReader::new(File::open(filename)?))), ③
    }
}
```

① 该函数将接受文件名，并返回一个错误或一个实现了 BufRead trait 的装箱值。

② 当文件名是“-”时，从 std::io::stdin 读取。

③ 否则，使用 File::open 尝试打开给定文件或传播错误。

如果 File::open 成功，结果将是一个文件句柄，它是读取文件内容的机制。文件句柄和
std::io::stdin 都实现了 BufRead trait，这意味着这些值例如会响应 BufRead::lines 函数来生成文本行。请注意，BufRead::lines 会删除行结束符，例如 Windows 上的 \r\n 和 Unix 上的 \n。

你再次看到我使用 Box 来创建一个指向堆分配内存的指针以保存文件句柄。你可能会想知道这是否完全必要。我可以尝试不使用 Box 编写该函数：

```rust
// This will not compile
fn open(filename: &str) -> MyResult<dyn BufRead> {
    match filename {
        "-" => Ok(BufReader::new(io::stdin())),
        _ => Ok(BufReader::new(File::open(filename)?)),
    }
}
```

如果我尝试编译这段代码，我会得到以下错误：

```text
error[E0277]: the size for values of type `(dyn std::io::BufRead + 'static)`
cannot be known at compilation time
  --> src/lib.rs:88:28
   |
88 | fn open(filename: &str) -> MyResult<dyn BufRead> {
   |                         ^^^^^^^^^^^^^^^^^^^^^^^^
   |                         doesn't have a size known
   |                         at compile-time
   |
   = help: the trait `Sized` is not implemented for `(dyn std::io::BufRead + 'static)`
```

正如编译器所说，BufRead 没有为 Sized trait 实现。如果一个变量没有固定的已知大小，那么 Rust 就无法将其存储在栈上。解决方案是改为在堆上分配内存，将返回值放入 Box 中，Box 是一个大小已知的指针。

前面的 open 函数非常紧凑。我可以理解如果你认为这有点复杂；然而，它基本能处理你会遇到的任何错误。为了演示这一点，将你的 run 改为如下：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files { ①
        match open(&filename) { ②
            Err(err) => eprintln!("Failed to open {}: {}", filename, err), ③
            Ok(_) => println!("Opened {}", filename), ④
        }
    }
    Ok(())
}
```

- ① 遍历文件名。
- ② 尝试打开文件名。注意使用 & 来借用变量。
- ③ 当 open 失败时向 STDERR 打印错误消息。
- ④ 当 open 成功时打印成功消息。

尝试使用以下内容运行你的程序：

- 1. 一个有效输入文件
- 2. 一个不存在的文件
- 3. 一个无法读取的文件

对于最后一个选项，你可以像这样创建一个无法读取的文件：

```bash
$ touch cant-touch-this && chmod 000 cant-touch-this
```

运行你的程序并验证你的代码能优雅地打印错误输入文件的错误信息并继续处理有效的文件：

```
$ cargo run -- blargh cant-touch-this tests/inputs/fox.txt
Failed to open blargh: No such file or directory (os error 2)
Failed to open cant-touch-this: Permission denied (os error 13)
Opened tests/inputs/fox.txt
```

加入此项后，你应该能够通过 **cargo test skips_bad_file**。现在你能够打开和读取有效输入文件了，我希望你自己完成程序。你能想出如何逐行读取已打开的文件吗？从仅有一行的 *tests/inputs/fox.txt* 开始。你应该能看到以下输出：

```
$ cargo run -- tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

验证你是否可以默认读取 `STDIN`。在以下命令中，我将使用 `|` 将第一个命令的 `STDOUT` 管道传输到第二个命令的 `STDIN`：

```
$ cat tests/inputs/fox.txt | cargo run
The quick brown fox jumps over the lazy dog.
```

当提供文件名“-”时，输出应相同。在以下命令中，我将使用 `bash` 重定向操作符 `<` 从给定文件名获取输入并提供给 `STDIN`：

```
$ cargo run -- - < tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

接下来，尝试一个有多行输入的文件并尝试为 `-n` 编号行：

```
$ cargo run -- -n tests/inputs/spiders.txt
    1  Don't worry, spiders,
    2  I keep house
    3  casually.
```

然后尝试为 -b 跳过空白行的编号：

```
$ cargo run -- -b tests/inputs/the-bustle.txt
    1  The bustle in a house
    2  The morning after death
    3  Is solemnest of industries
    4  Enacted upon earth,–

    5  The sweeping up the heart,
    6  And putting love away
    7  We shall not want to use again
    8  Until eternity.
```

经常运行 cargo test 来查看哪些测试失败。tests/cli.rs 中的测试与第 2 章类似，但我添加了一些更清晰的组织。例如，我在该模块顶部定义了几个常量 &str 值，并在整个 crate 中使用它们。我使用 ALL_CAPS 名称的常见约定来突出它们在整个 crate 中可见或作用域：

```
const PRG: &str = "catr";
const EMPTY: &str = "tests/inputs/empty.txt";
const FOX: &str = "tests/inputs/fox.txt";
const SPIDERS: &str = "tests/inputs/spiders.txt";
const BUSTLE: &str = "tests/inputs/the-bustle.txt";
```

为了测试程序在给定不存在文件时是否会退出，我使用 rand crate 生成一个随机不存在的文件名。对于以下函数，我将使用 rand:: {distributions::Alphanumeric, Rng} 导入此函数中需要的 crate 的各个部分：

```
fn gen_bad_file() -> String { ①
    loop { ②
```

```
let filename: String = rand::thread_rng()
    .sample_iter(&Alphanumeric)
    .take(7)
    .map(char::from)
    .collect();

if fs::metadata(&filename).is_err() {
    return filename;
}
}
```

- 1. 该函数将返回一个 `String`，它是一个动态生成的字符串，与我一直在使用的 `str` 结构体密切相关。
- 2. 开始一个无限 `loop`。
- 3. 创建一个由七个字母数字字符组成的随机字符串。
- 4. `fs::metadata` 在给定文件名不存在时返回错误，因此返回不存在的文件名。

## NOTE

在前面的函数中，我在创建后两次使用 `filename`。第一次，我使用 `&filename` 借用它，第二次我没有使用 &。尝试删除 & 并运行代码。你应该会得到一个错误消息，指出 `filename` 值的所有权已移入 `fs::metadata`。有效地，该函数消耗了该值，使其无法使用。& 表示我只想借用对该值的引用。

```
error[E0382]: use of moved value: `filename`
  --> tests/cli.rs:37:20
  |
30 |         let filename: String = rand::thread_rng()
  |         ---------------------- move occurs because `filename`
has type `String`,
  |         which does not implement the
`Copy` trait
```

```
...
36 |     if fs::metadata(filename).is_err() {
|         ------- value moved here
37 |     return filename;
|         ^^^^^^^^ value used here after move
```

不用担心如果您还不完全理解前面的代码。我只是展示它，以便您了解它在 `skips_bad_file` 测试中的使用方式：

```
#[test]
fn skips_bad_file() -> TestResult {
    let bad = gen_bad_file(); ①
    let expected = format!("{}: .* [()]os error 2[]]", bad);
②
    Command::cargo_bin(PRG)? ③
        .arg(&bad)
        .assert()
        .success() ④
        .stderr(predicate::str::is_match(expected)?);
    Ok(())
}
```

- ① 生成一个不存在的文件名。
- 
- ② 预期的错误消息应包含文件名以及在 Windows 或 Unix 平台上都出现的字符串“os error 2”。
- 
- ③ 使用错误文件运行程序并验证 STDERR 是否与预期模式匹配。
- ④ 命令应成功，因为错误文件只应生成警告而不应终止进程。

## 提示

在前面的函数中，我使用了 `format!` 宏来生成一个新的 `String`。该宏的工作方式类似于 `print!`，不同之处在于它返回值而不是打印它。

我创建了一个 `run` 辅助函数来使用输入参数运行程序并验证输出是否与 `mk-outs.sh` 生成的文件中的文本匹配：

```rust
fn run(args: &[&str], expected_file: &str) -> TestResult {
    let expected = fs::read_to_string(expected_file)?;
    Command::cargo_bin(PRG)?
        .args(args)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

- 1. 该函数接受一个 `&str` 参数切片和包含预期输出的文件名。函数返回 `TestResult`。
- 2. 尝试读取预期输出文件。
- 3. 使用参数执行程序并验证其成功运行并产生预期输出。

我像这样使用此函数：

```rust
#[test]
fn bustle() -> TestResult {
    run(&[BUSTLE], "tests/expected/the-bustle.txt.out")
}
```

① 使用 BUSTLE 输入文件运行程序并验证输出是否与 mk-outs.sh 产生的输出匹配。

我还编写了一个辅助函数通过 STDIN 提供输入：

```
fn run_stdin(
    input_file: &str, ①
    args: &[&str],
    expected_file: &str,
) -> TestResult {
    let input = fs::read_to_string(input_file)?; ②
    let expected = fs::read_to_string(expected_file)?;
    Command::cargo_bin(PRG)? ③
        .args(args)
        .write_stdin(input)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

① 第一个参数是包含应提供给 STDIN 的文本的文件名。

② 尝试读取输入和预期文件。

③ 尝试使用给定参数和 STDIN 运行程序并验证输出。

此函数的使用方式类似：

```
#[test]
fn bustle_stdin() -> TestResult {
    run_stdin(BUSTLE, &["-"], "tests/expected/the-bustle.txt.stdin.out") ①
}
```

使用给定文件名的内容作为 STDIN 并使用输入文件名 “-” 运行程序。验证输出是否与预期值匹配。

这应该足以开始。出发吧！完成后回来。

## 解决方案

我希望你觉得编写这个程序很有趣且具有挑战性。逐步处理复杂程序很重要。我将向你展示我是如何以这种方式构建我的程序的。

## 读取文件中的行

我从打印打开的文件句柄的行开始：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                for line_result in file.lines() {
                    let line = line_result?;
                    println!("{}", line);
                }
            }
        }
    }
    Ok(())
}
```

1 打开文件时出现问题时打印文件名和错误。

2 遍历来自 BufRead::lines 的每个 line_result 值。

- ③ 从 line_result 解包 Ok 值或传播错误。
- ④ 打印该行。

## 注意

读取文件中的行时，您不会直接从文件句柄获取行，而是获取一个 std::io::Result，它是一种“在 std::io 中广泛用于可能产生错误的任何操作的”类型。读写文件属于 IO（输入/输出）范畴，依赖于操作系统和文件系统等外部资源。虽然从文件句柄读取一行失败的可能性不大，但关键是它可能会失败。

如果你运行 cargo test，你应该能通过大约一半的测试，对于这么少的代码行来说，这已经不错了。

## 打印行号

接下来，我想为 -n|--number 选项添加行号打印。一种 C 程序员可能熟悉的解决方案如下所示：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                let mut line_num = 0; ①
                for line_result in file.lines() {
                    let line = line_result?;
                    line_num += 1; ②

                    if config.number_lines { ③
                        println!("{:>6}\t{}", line_num,
line); ④
                    } else {
println!("{} ", line); ⑤
}
}
}
}
Ok(())
}
```

- ① 初始化一个可变计数器变量来保存行号。
- ② 将 1 加到行号上。
- ③ 检查是否打印行号。
- ④ 如果是，则在宽度为 6 个字符的右对齐字段中打印当前行号，后跟制表符，然后是文本行。
- ⑤ 否则，打印该行。

请记住，Rust 中的所有变量默认都是不可变的，因此需要为 line_num 添加 mut，因为我打算更改它。+= 运算符是复合赋值，它将右侧值 1 加到 line_num 上以递增它¹。值得注意的是，格式语法 {:>6} 表示字段宽度为六个字符且文本右对齐。（你可以使用 < 进行左对齐，^ 进行居中对齐。）这种语法类似于 C、Perl 和 Python 字符串格式化的 printf。

如果我运行此版本的程序，看起来相当不错：

```
$ cargo run -- tests/inputs/spiders.txt -n
    1  Don't worry, spiders,
    2  I keep house
    3  casually.
```

虽然这已经足够好用，但我想指出一种使用 `Iterator::enumerate` 的更地道的解决方案。此方法将返回一个元组，其中包含 *可迭代对象* 中每个元素的索引位置和值，*可迭代对象* 是指可以一直产生值直到耗尽的东西：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                for (line_num, line_result) in
                file.lines().enumerate() { ①
                    let line = line_result?;
                    if config.number_lines {
                        println!("{:>6}\t{}", line_num + 1,
                        line); ②
                    } else {
                        println!("{}", line);
                    }
                }
            }
        }
    }
    Ok(())
}
```

① 来自 `Iterator::enumerate` 的元组值可以使用模式匹配解包。

② `enumerate` 的编号从 0 开始，因此加 1 以模仿从 1 开始的 `cat`。

这将产生相同的输出，但现在代码避免使用可变值。我可以执行 `cargo test fox` 来运行所有以 fox 开头的测试，我发现其中两个通过了三个。程序在 -b 标志上失败，因此接下来我需要处理仅对非空白行打印行号。请注意，在此版本中，我还将删除 `line_result` 并对 `line` 变量进行遮蔽：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                let mut last_num = 0; ①
                for (line_num, line) in
                file.lines().enumerate() {
                    let line = line?; ②
                    if config.number_lines { ③
                        println!("{:>6}\t{}", line_num + 1,
                        line);
                    } else if config.number_nonblank_lines
                    { ④
                        if !line.is_empty() {
                            last_num += 1;
                            println!("{:>6}\t{}", last_num,
                            line); ⑤
                        } else {
                            println!(); ⑥
                        }
                    } else {
                        println!("{}", line); ⑦
                    }
                }
            }
        }
    }
    Ok(())
}
```

- ① 为最后一个非空白行的编号初始化一个可变变量。
- ② 用解包 Result 的结果对 line 进行遮蔽。
- ③ 处理行号打印。
- ④ 处理非空白行的行号打印。
- ⑤ 如果行不为空，则递增 last_num 并打印输出。
- ⑥ 如果行为空，则打印一个空白行。
- ⑦ 如果没有编号选项，则打印该行。

> **注意**
> 
> 在 Rust 中 *遮蔽* 一个变量是指重用变量的名称并将其设置为新值。可以说 `line_result/line` 代码可能更明确、更易读，但在这种上下文中重用 `line` 是你更可能遇到的更地道的 Rust 代码。

如果运行 `cargo test`，你应该能通过所有测试。

## 更进一步

你现在有一个可工作的程序，但不必就此停止。如果想接受额外的挑战，尝试实现手册页中 BSD 和 GNU 版本所示的其他选项即可。对每个选项，使用 `cat` 创建预期的输出文件，然后扩展测试以检查你的程序是否创建了相同的输出。我还建议你查看 `bat`，它是一个 Rust 编写的 `cat` 克隆版（“带翅膀的”），实现更为完整。

`cat -n` 的编号行输出在某些方面与 `nl` 相似，后者是一个“行号过滤器”程序。`cat` 也与逐页或逐屏显示文本的程序有点相似，这类程序被称为 *pagers*，例如 `more` 和 `less`。 (`more` 会显示一页文本，并在底部显示“More”以提示你可以继续。显然有人想出了巧妙的主意并将自己的克隆命名为 `less`，但它做的事情是一样的。) 考虑实现这两个程序。阅读手册页，创建测试输出，并从本项目中复制思路来编写和测试你的版本。

## 总结

你在本章取得了长足进步，创建了一个更复杂的程序。考虑一下你学到了什么：

- 你将代码分离为库（`src/lib.rs`）和二进制（`src/main.rs`）crate，这有助于更轻松地组织和封装思想。
- 你创建了第一个 `struct`，它有点像其他语言中的类声明。这个结构体让你能够创建一个名为 `Config` 的复杂数据结构，用来描述程序的输入。
- 默认情况下，所有值和函数都是不可变且私有的。你学会了使用 `mut` 使值可变，使用 `pub` 使值或函数公开。
- 你采用了测试先行的[content]
```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                let mut last_num = 0; ①
                for (line_num, line) in
                file.lines().enumerate() {
                    let line = line?; ②
                    if config.number_lines { ③
                        println!("{:>6}\t{}", line_num + 1,
                        line);
                    } else if config.number_nonblank_lines
                    { ④
                        if !line.is_empty() {
                            last_num += 1;
                            println!("{:>6}\t{}", last_num,
                            line); ⑤
                        } else {
                            println!(); ⑥
                        }
                    } else {
                        println!("{}", line); ⑦
                    }
                }
            }
        }
    }
    Ok(())
}
```

- ① 为最后一个非空白行的编号初始化一个可变变量。
- ② 用解包 Result 的结果对 line 进行遮蔽。
- ③ 处理行号打印。
- ④ 处理非空白行的行号打印。
- ⑤ 如果行不为空，则递增 last_num 并打印输出。
- ⑥ 如果行为空，则打印一个空白行。
- ⑦ 如果没有编号选项，则打印该行。

> **注意**
> 
> 在 Rust 中 *遮蔽* 一个变量是指重用变量的名称并将其设置为新值。可以说 `line_result/line` 代码可能更明确、更易读，但在这种上下文中重用 `line` 是你更可能遇到的更地道的 Rust 代码。

如果运行 `cargo test`，你应该能通过所有测试。

## 更进一步

你现在有一个可工作的程序，但不必就此停止。如果想接受额外的挑战，尝试实现手册页中 BSD 和 GNU 版本所示的其他选项即可。对每个选项，使用 `cat` 创建预期的输出文件，然后扩展测试以检查你的程序是否创建了相同的输出。我还建议你查看 `bat`，它是一个 Rust 编写的 `cat` 克隆版（“带翅膀的”），实现更为完整。

`cat -n` 的编号行输出在某些方面与 `nl` 相似，后者是一个“行号过滤器”程序。`cat` 也与逐页或逐屏显示文本的程序有点相似，这类程序被称为 *pagers*，例如 `more` 和 `less`。 (`more` 会显示一页文本，并在底部显示“More”以提示你可以继续。显然有人想出了巧妙的主意并将自己的克隆命名为 `less`，但它做的事情是一样的。) 考虑实现这两个程序。阅读手册页，创建测试输出，并从本项目中复制思路来编写和测试你的版本。

## 总结

你在本章取得了长足进步，创建了一个更复杂的程序。考虑一下你学到了什么：

- 你将代码分离为库（`src/lib.rs`）和二进制（`src/main.rs`）crate，这有助于更轻松地组织和封装思想。
- 你创建了第一个 `struct`，它有点像其他语言中的类声明。这个结构体让你能够创建一个名为 `Config` 的复杂数据结构，用来描述程序的输入。
- 默认情况下，所有值和函数都是不可变且私有的。你学会了使用 `mut` 使值可变，使用 `pub` 使值或函数公开。
- 你采用了测试先行的方法，所有测试在程序编写之前就已存在。当程序通过所有测试时，你可以确信程序满足了测试中编码的所有规范。
- 你看到了如何使用 `rand` crate 为不存在的文件生成随机字符串。
- 你弄明白了如何从 `STDIN` 或普通文件读取文本行。
- 你使用了 `eprintln!` 宏向 `STDERR` 打印，以及 `format!` 动态生成新字符串。
- 你使用 `for` 循环遍历可迭代对象中的每个元素。
- 你发现 `Iterator::enumerate` 方法会以元组形式返回索引和元素，这对于给文本行编号很有用。
- 你学会了使用指向文件句柄的 `Box` 来读取 `STDIN` 或普通文件。

在下一章，你将学习更多关于按行、字节或字符读取文件的内容。

1 请注意 Rust 没有一元 ++ 运算符，因此你不能使用 `line_num++` 来将变量递增 1。
