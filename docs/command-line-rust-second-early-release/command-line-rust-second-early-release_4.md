# 第4章 头痛

## 早期发布读者的说明

使用 Early Release 电子书，你可以在这些技术正式发布之前就获得书籍的最早形式——作者在写作时的原始未编辑内容，因此你可以在这些标题的正式发布之前就利用这些技术。

这将是最终书籍的第 4 章。请注意 GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系作者 kyClark@gmail.com。

> *换个角度站立在自己的头上，给我一些属于我自己的皮肤*
> 
> —They Might Be Giants

本章的挑战是实现 `head` 程序，它将打印一个或多个文件的前几行或字节。这是一种窥探普通文本文件内容的好方法，通常比 `cat` 更合适的选择。当面对某个过程的输出文件目录时，这是一种快速扫描潜在问题的好方法。

在本练习中，你将学习：

- 如何创建接受值的可选命令行参数
- 如何将字符串解析为数字
- 如何编写和运行单元测试
- 如何在 `match` 分支中使用守卫
- 如何使用 `From`、`Into` 和 `as` 在类型之间转换
- 如何在迭代器或文件句柄上使用 `take`
- 如何在读取文件句柄时保留行结束符
- 如何从文件句柄读取字节

## head 的工作原理

你应该记住，原版 AT&T Unix 操作系统有许多实现，例如 BSD（Berkeley Standard Distribution）、SunOS/Solaris、HP-UX 和 Linux。这些操作系统大多数都有一个 `head` 程序的版本，默认显示一个或多个文件的前十行。大多数可能都有 `-n` 选项来控制显示的行数，以及 `-c` 选项来显示一定数量的字节。BSD 版本只有这两个选项，我可以通过 `man head` 看到：

```
HEAD(1)                                 BSD General Commands Manual
HEAD(1)

NAME
        head -- display first lines of a file

SYNOPSIS
        head [-n count | -c bytes] [file ...]

DESCRIPTION
        This filter displays the first count lines or bytes of
each of the speci-
        fied files, or of the standard input if no files are
specified.  If count
        is omitted it defaults to 10.

        If more than a single file is specified, each file is
preceded by a
    header consisting of the string '==> XXX <==' where
'XXX' is the name
    of the file.
```

## 退出状态

head 实用程序在成功时退出 0，在发生错误时退出 >0。

## 另请参阅

tail(1)

## 历史

head 命令出现在 PWB UNIX 中。

BSD
June 6, 1993
BSD

## 使用 GNU 版本时，我可以运行 head --help 来阅读用法：

```
Usage: head [OPTION]... [FILE]...
Print the first 10 lines of each FILE to standard output.
With more than one FILE, precede each with a header giving
the file name.
With no FILE, or when FILE is -, read standard input.

Mandatory arguments to long options are mandatory for short
options too.
  -c, --bytes=[-]K          print the first K bytes of each
file;
                          with the leading '-', print
all but the last
                          K bytes of each file
  -n, --lines=[-]K          print the first K lines instead
of the first 10;
                          with the leading '-', print
all but the last
                          K lines of each file
  -q, --quiet, --silent     never print headers giving file
names
  -v, --verbose             always print headers giving file
names
      --help               display this help and exit
      --version            output version information and exit
```

K 可能带有乘数后缀：
b 512, kB 1000, K 1024, MB 1000*1000, M 1024*1024,
GB 1000*1000*1000, G 1024*1024*1024, 以及 T、P、E、
Z、Y 等。

请注意 GNU 版本能够使用负数指定 -n 和 -c 并使用 K、M 等后缀，我将不会实现此功能。在两个版本中，文件都是可选的位置参数，默认读取 STDIN 或当文件名是 "-" 时读取 STDIN。-n 和 -c 是可选参数，接受整数值。

为了演示使用 head 的一些示例，我将使用 04_headr/tests/inputs 中找到的文件。给定一个空文件，没有输出，你可以使用 head tests/inputs/empty.txt 来验证。默认情况下，head 会打印文件的前 10 行。如果文件少于 10 行，它会打印所有行。你可以使用 tests/inputs/three.txt 来查看，它有 3 行：

```
$ cd 04_headr
$ head tests/inputs/three.txt
Three
lines,
four words.
```

-n 选项允许你控制显示的行数。例如，我可以使用以下命令只选择 2 行：

```
$ head -n 2 tests/inputs/three.txt
Three
lines,
```

-c 选项仅显示文件中的指定字节数，例如仅前 4 个字节：

```
$ head -c 4 tests/inputs/three.txt
Thre
```

奇怪的是，GNU 版本允许你同时提供 -n 和 -c 并默认显示字节。BSD 版本会拒绝这两个参数：

```
$ head -n 1 -c 2 tests/inputs/one.txt
head: can't combine line and byte counts
```

-n 或 -c 的任何非正整数值都会生成错误并使程序停止，并且错误会回显非法值：

```
$ head -n 0 tests/inputs/one.txt
head: illegal line count -- 0
$ head -c foo tests/inputs/one.txt
head: illegal byte count -- foo
```

当存在多个参数时，head 会添加标题并在每个文件之间插入空白行：

```
$ head -n 1 tests/inputs/*.txt
==> tests/inputs/empty.txt <==

==> tests/inputs/one.txt <==
One line, four words.

==> tests/inputs/three.txt <==
Three

==> tests/inputs/two.txt <==
Two lines.
```

没有文件参数时，head 会从 STDIN 读取：

```
$ cat tests/inputs/three.txt | head -n 2
Three
lines,
```

与第 3 章中的 cat 类似，任何不存在或无法读取的文件都会被跳过，并向 STDERR 打印警告。在以下命令中，我将使用 *blargh* 作为不存在的文件，并创建一个名为 *cant-touch-this* 的无法读取的文件：

```
$ touch cant-touch-this && chmod 000 cant-touch-this
$ head blargh cant-touch-this tests/inputs/one.txt
head: blargh: No such file or directory
head: cant-touch-this: Permission denied
==> tests/inputs/one.txt <==
One line, four words.
```

这将是挑战程序预计要重现的全部内容。

## 开始

你可能已经预料到我希望你编写的程序将被称为 `headr`（发音为 *head-er*）。首先运行 `cargo new headr` 并将我的 `04_headr/tests` 目录复制到你的项目目录中。向你的 *Cargo.toml* 添加以下依赖项：

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

我建议你再次将源代码拆分，使 *src/main.rs* 看起来像这样：

```
fn main() {
    if let Err(e) = headr::get_args().and_then(headr::run)
    {
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```

在 `src/lib.rs` 中开始，通过引入 `clap` 和 `Error` trait 并声明 `MyResult`，你可以从第 3 章的源代码中复制：

```rust
use clap::{App, Arg};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;
```

程序将有三个参数，可以用 `Config` 结构体表示：

```rust
#[derive(Debug)]
pub struct Config {
    files: Vec<String>, ①
    lines: usize, ②
    bytes: Option<usize>, ③
}
```

① `files` 将是一个字符串向量。

② 要打印的 `lines` 数量的类型为 `usize`。

③ `bytes` 将是一个可选的 `usize`。

原始类型 `usize` 是“指针大小的无符号整数类型”，其大小在 32 位操作系统上为 4 字节，在 64 位上为 8 字节。选择 `usize` 有点随意，因为我只是想存储某种正整数。我也可以使用 `u32`（无符号 32 位整数）或 `u64`（无符号 64 位整数），但我确实想要一个 *unsigned* 类型，因为它只表示正整数值。如果我想允许像 GNU 版本那样使用负值，就需要使用 `i32` 或 `i64` 这样的 *signed* 整数来表示正数或负数。

lines 和 bytes 将在几个函数中使用，其中一个期望 usize，另一个期望 u64。这将提供一个机会稍后讨论如何转换类型。你的程序应将 lines 的默认值设为 10，但 bytes 将是 Option，我在第 2 章中首次介绍过。这意味着 bytes 将在用户提供有效值时为 Some<usize>，如果未提供则为 None。

你可以从以下大纲开始 get_args 函数。你需要添加代码来解析参数并返回 Config 结构体：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("headr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust head")
        ... // what goes here?
        .get_matches();

    Ok(Config {
        files: ...
        lines: ...
        bytes: ...
    })
}
```

### TIP

此程序的所有命令行参数都是可选的，因为文件将默认为 "-"，lines 将默认为 10，bytes 可以省略。第 3 章中的可选参数是标志，但这里 lines 和 bytes 需要将 Arg::takes_value 设置为 true。

你可以先用打印配置的 run 函数开始：

```
pub fn run(config: Config) -> MyResult<()> {
    println!("{:#?}", config); ①
    Ok(()) ②
}
```

- ① 美观地打印 `config`。你也可以使用 `dbg!(config)`。
- ② 返回成功的结果。

## 解析字符串为数字

`clap` 返回的所有值都是字符串，但当存在时你需要将 `lines` 和 `bytes` 转换为整数。我将向你展示如何为此使用 `str::parse`。此函数将返回一个 `Result`，当提供的值无法解析为数字时为 `Err`，或包含转换后数字的 `Ok`。我将编写一个名为 `parse_positive_int` 的函数，尝试将字符串值解析为正的 `usize` 值。你可以将其添加到 `src/lib.rs`：

```
fn parse_positive_int(val: &str) -> MyResult<usize> { ①
    unimplemented!(); ②
}
```

- ① 此函数接受一个 `&str` 并将返回一个正的 `usize` 或错误。
- ② `unimplemented!` 宏“通过带有未实现消息的 panic 指示未实现的代码。”

为了遵循测试驱动开发的精神，我将为此函数添加一个 *unit* 测试。我建议将其放在测试的函数之后：

```
#[test]
fn test_parse_positive_int() {
    // 3 is an OK integer
let res = parse_positive_int("3");
assert!(res.is_ok());
assert_eq!(res.unwrap(), 3);

// Any string is an error
let res = parse_positive_int("foo");
assert!(res.is_err());
assert_eq!(res.unwrap_err().to_string(),
"foo".to_string());

// A zero is an error
let res = parse_positive_int("0");
assert!(res.is_err());
assert_eq!(res.unwrap_err().to_string(),
"0".to_string());
}
```

运行 `cargo test parse_positive_int` 并验证测试确实失败。现在停止阅读并编写一个通过此测试的函数版本。我会在这里等你完成。

```
TIME PASSES.
AUTHOR GETS A CUP OF TEA AND CONSIDERS HIS LIFE CHOICES.
AUTHOR RETURNS TO THE NARRATIVE.
```

怎么样？我想很棒！以下是通过前面测试的我编写的函数：

```
fn parse_positive_int(val: &str) -> MyResult<usize> {
    match val.parse() { ①
        Ok(n) if n > 0 => Ok(n), ②
        _ => Err(From::from(val)), ③
    }
}
```

- ① 尝试解析给定的值。Rust 根据返回类型推断 `usize` 类型。
- ② 如果解析成功且解析值 n 大于 0，则将其作为 Ok 变体返回。



③ 对于任何其他结果，返回一个包含给定值的 Err。

我到目前为止已经多次使用过 `match`，但这是第一次展示 `match` 分支可以包含一个 *守卫*，它是在模式匹配之后进行的额外检查。不知道你怎么想，但我觉得这挺酷的。

## 将字符串转换为错误

当我无法将给定的字符串值解析为正整数时，我希望返回原始字符串，以便将其包含在错误消息中。为此，在前面的函数中，我使用了冗余命名的 `From::from` 函数将输入的 `&str` 值转换为 `Error`。请考虑这个版本，我尝试直接将无法解析的字符串放入 `Err` 中：

```
```rust
fn parse_positive_int(val: &str) -> MyResult<usize> {
    match val.parse() {
        Ok(n) if n > 0 => Ok(n),
        _ => Err(val), // This will not compile
    }
}
```
```

如果我尝试编译它，会得到以下错误：

```
```
error[E0308]: mismatched types
  --> src/lib.rs:75:18

75 |         _ => Err(val), // This will not compile
   |                ^^^^
   |                |
   |                expected struct `Box`, found `&str`
   |                help: store this in the heap by
calling `Box::new`:
   |                `Box::new(val)`
   |
   = note: expected struct `Box<dyn std::error::Error>`
            found reference `&str`
```
```

```
= note: for more on the distinction between the stack
and the heap,
    read https://doc.rust-lang.org/book/ch15-01-box.html,
    https://doc.rust-lang.org/rust-by-example/std/box.html,
    and https://doc.rust-lang.org/std/boxed/index.html
```

问题是我期望返回一个 MyResult，它被定义为 Ok<T>（T 为任意类型）或实现 Error trait 且存储在 Box 中的某物：

```
type MyResult<T> = Result<T, Box<dyn Error>>;
```

在前面的代码中，&str 既不实现 Error 也不存在于 Box 中。我可以根据建议尝试通过将其更改为 Err(Box::new(val)) 来修复它。不幸的是，这仍然无法编译，因为我仍然没有满足 Error trait：

```
error[E0277]: the trait bound `str: std::error::Error` is
not satisfied
    --> src/lib.rs:75:18
    |
75 |           => Err(Box::new(val)), // This will not
compile
    |                  ^^^^^^^^^^^^^^ the trait
`std::error::Error` is not
    |                          implemented for `str`
    |
    = note: required because of the requirements on the impl
    of
    `std::error::Error` for `&str`
    = note: required for the cast to the object type `dyn
std::error::Error`
```

进入 std::convert::From trait，它有助于从一种类型转换为另一种类型。例如，文档展示了如何从 str 转换为 String：

```
let string = "hello".to_string();
let other_string = String::from("hello");
```

```
assert_eq!(string, other_string);
```

在我的例子中，我可以使用 `std::convert::From` 和 `std::convert::Into` 以几种方式将 `&str` 转换为 `Error`。正如文档所述：

> `From` 在执行错误处理时也非常有用。当构建一个可能失败的函数时，返回类型通常为 `Result<T, E>` 的形式。`From` trait 通过允许函数返回封装多种错误类型的单一错误类型来简化错误处理。

图 4-1 显示了几种等价的写法，没有哪一种更优。

```
fn parse_positive_int(val: &str) -> MyResult<usize> {
    match val.parse() {
        Ok(n) if n > 0 => Ok(n),
        _ => Err(From::from(val)),
    }
}

or
Err(val.into())
Err(Into::into(val))
```

图 4-1. 使用 From 和 Into trait 将 &str 转换为 Error 的替代方法

现在你已经有了一种将字符串转换为数字的方法，将其集成到你的 get_args 中。看看是否能让你的程序打印出如下的用法。注意我使用了 GNU 版本的短名称和长名称：

```
$ cargo run -- -h
headr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust head

USAGE:
    headr [OPTIONS] <FILE>...
```

## 标志：

- `-h, --help` 打印帮助信息
- `-V, --version` 打印版本信息

## 选项：

- `-c, --bytes <BYTES>` 字节数
- `-n, --lines <LINES>` 行数 [默认值: 10]

## 参数：

- `<FILE>...` 输入文件 [默认值: -]

运行程序且不带输入，并验证默认值是否正确设置：

```
$ cargo run
Config {
  files: [ ①
    "-",
  ],
  lines: 10, ②
  bytes: None, ③
}
```

① files 应默认使用文件名 “-”。

② 行数应默认设置为 10。

③ bytes 应为 None。

使用参数运行程序并确保它们被正确解析：

```
$ cargo run -- -n 3 tests/inputs/one.txt
Config {
  files: [
    "tests/inputs/one.txt", ①
  ],
  lines: 3, ②
  bytes: None, ③
}
```

- 1. 位置参数 *tests/inputs/one.txt* 被解析为 `files` 之一。
- 2. `lines` 的 `-n` 选项将其设置为 3。
- 3. `bytes` 的 `-b` 选项默认为 `None`。

如果我提供多个位置参数，它们都将进入 `files`，而 `-c` 参数将进入 `bytes`。在下面的命令中，我再次依赖 `bash` shell 将文件 glob `*.txt` 展开为所有以 `.txt` 结尾的文件。PowerShell 用户应参考第 3 章中所示的等效 `Get-ChildItem` 用法：

```
$ cargo run -- -c 4 tests/inputs/*.txt
Config {
    files: [
        "tests/inputs/empty.txt", ①
        "tests/inputs/one.txt",
        "tests/inputs/three.txt",
        "tests/inputs/two.txt",
    ],
    lines: 10, ②
    bytes: Some( ③
        4,
    ),
}
```

- 1. 有四个以 `.txt` 结尾的文件。
- 2. `lines` 仍设置为默认值 10。
- 3. `-c 4` 导致 `bytes` 现在为 `Some(4)`。

任何无法解析为正整数的 `-n` 或 `-c` 值都应导致程序以错误终止：

```
$ cargo run -- -n blarg tests/inputs/one.txt
illegal line count -- blarg
$ cargo run -- -c 0 tests/inputs/one.txt
illegal byte count -- 0
```

程序应禁止同时出现 -n 和 -c：

```
$ cargo run -- -n 1 -c 1 tests/inputs/one.txt
error: The argument '--lines <LINES>' cannot be used with '--bytes <BYTES>'
```

仅仅解析和验证参数就是一个挑战，但我知道你能做到。在弄清楚的过程中请务必查阅 clap 文档。我建议你在程序能够通过 cargo test dies 中包含的所有测试之前不要继续前进：

```
running 3 tests
test dies_bad_lines ... ok
test dies_bad_bytes ... ok
test dies_bytes_and_lines ... ok
```

## 定义参数

下面是我为 clap 定义参数的方式。注意 lines 和 bytes 这两个选项将接受值。这与第 3 章中作为布尔值使用的标志不同：

```
let matches = App::new("headr")
    .version("0.1.0")
    .author("Ken Youens-Clark <kyclark@gmail.com>")
    .about("Rust head")
    .arg(
        Arg::with_name("lines") ①
            .short("n")
            .long("lines")
            .value_name("LINES")
            .help("Number of lines")
            .default_value("10"),
    )
    .arg(
Arg::with_name("bytes") ②
    .short("c")
    .long("bytes")
    .value_name("BYTES")
    .takes_value(true)
    .conflicts_with("lines")
    .help("Number of bytes"),
)
.arg(
    Arg::with_name("files") ③
    .value_name("FILE")
    .help("Input file(s)")
    .required(true)
    .default_value("-")
    .min_values(1),
)
.get_matches();
```

① `lines` 选项接受一个值，默认为 “10”。

② `bytes` 选项接受一个值，并且与 `lines` 参数冲突，因此它们互斥。

③ `files` 参数是位置参数，必需，接受一个或多个值，默认为 “-”。

### 注意

> `Arg::value_name` 将在用法文档中打印，因此请确保选择一个描述性的名称。不要将其与 `Arg::with_name` 混淆，后者唯一定义了在代码中访问参数的名称。

下面是我如何在 `get_args` 中使用 `parse_positive_int` 来验证 `lines` 和 `bytes`。当函数返回 `Err` 变体时，我使用 `?` 将错误传播到 `main` 并结束程序；否则，我返回 `Config`：


```rust
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("head")... // Same as before

    let lines = matches
        .value_of("lines") ①
        .map(parse_positive_int) ②
        .transpose() ③
        .map_err(|e| format!("illegal line count -- {}", e))?; ④

    let bytes = matches ⑤
        .value_of("bytes")
        .map(parse_positive_int)
        .transpose()
        .map_err(|e| format!("illegal byte count -- {}", e))?;

    Ok(Config {
        files: matches.values_of_lossy("files").unwrap(),
        ⑥
        lines: lines.unwrap(), ⑦
        bytes ⑧
    })
}
```

① ArgMatches.value_of 返回一个 Option<&str>。

② 使用 Option::map 从 Some 中解包 &str 并将其发送给 parse_positive_int。

③ Option::map 的结果将是一个 <Option<Result>>，而 Option::transpose 会将其转换为 <Result<Option>>。

④ 在出现 Err 的情况下，创建一个信息丰富的错误消息。使用 ? 来传播一个 Err 或解包 Ok 值。

⑤ 对 bytes 做同样的操作。

⑥ files 选项应至少有一个值，因此调用 Option::unwrap 是安全的。

- 7. `lines` 有默认值，因此安全地 unwrap。
- 8. `bytes` 应保留为 `Option`。由于字段名与变量名相同，使用 `struct` 字段初始化简写。

在前面的代码中，我本可以像这样为 `Config` 编写每一对键/值：

```rust
Ok(Config {
    files: matches.values_of_lossy("files").unwrap(),
    lines: lines.unwrap(),
    bytes: bytes,
})
```

Clippy 会建议如下：

```
$ cargo clippy
warning: redundant field names in struct initialization
  --> src/lib.rs:61:9
   |
61 |             bytes: bytes,
   |                    ^^^^^^^^ help: replace it with: `bytes`
   |
   = note: `#[warn(clippy::redundant_field_names)]` on by default
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#redundant_field_names
```

验证所有用户输入的工作量相当大，但现在我可以确信可以继续使用良好的数据。

## 处理输入文件

此挑战程序应像第 3 章那样处理输入文件，因此我建议你从那里引入 `open` 函数：

```rust
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

确保添加所有必需的依赖项：

```rust
use clap::{App, Arg};
use std::error::Error;
use std::fs::File;
use std::io::{self, BufRead, BufReader, Read};
```

扩展你的 run 函数以尝试打开文件，并在遇到错误时打印错误：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files { ①
        match open(&filename) { ②
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(_file) => println!("Opened {}", filename),
        }
    }
    Ok(())
}
```

- ① 遍历每个文件名。
- ② 尝试打开文件名。
- ③ 将错误打印到 STDERR。
- ④ 打印文件成功打开的消息。

使用一个好的文件和一个坏的文件运行你的程序，以确保它似乎能正常工作：

```
$ cargo run -- blargh tests/inputs/one.txt
blargh: No such file or directory (os error 2)
Opened tests/inputs/one.txt
```

接下来，尝试解决读取给定文件的行然后字节的问题，然后尝试添加分隔多个文件参数的头。仔细查看 head 处理无效文件时的错误输出。注意，可读文件首先有头然后是文件输出，但无效文件只打印错误。此外，有一个额外的空行分隔有效文件的输出：

```
$ head -n 1 tests/inputs/one.txt blargh
tests/inputs/two.txt
==> tests/inputs/one.txt <==
Öne line, four words.
head: blargh: No such file or directory

==> tests/inputs/two.txt <==
Two lines.
```

我特意为你设计了一些具有挑战性的输入以供考虑。要查看你面临的情况，请使用 file 命令报告文件类型信息：

```
$ file tests/inputs/*.txt
tests/inputs/empty.txt: empty ①
tests/inputs/one.txt:   UTF-8 Unicode text ②
tests/inputs/three.txt: ASCII text, with CRLF, LF line terminators ③
tests/inputs/two.txt:   ASCII text ④
```

① 这是一个空文件，只是为了确保你的程序不会崩溃。

② 该文件包含 Unicode，因为我在 Óne 中的 O 上加了变音符号，以迫使你考虑字节和

characters 的区别。

- 3. 该文件具有 Windows 风格的行尾。
- 4. 该文件具有 Unix 风格的行尾。

> **TIP**
> 在 Windows 上，换行符是回车和换行的组合，通常显示为 CRLF 或 \r\n。在 Unix 平台上，只使用换行符，即 LF 或 \n。这些行尾必须在你的程序输出中保留，因此你必须找到一种在不删除行尾的情况下读取文件行的方法。

## 读取字节与字符

我想解释从文件中读取 *bytes* 和 *characters* 之间的区别。在 20 世纪 60 年代初，美国信息交换标准代码（ASCII，读作 *as-key*）表中 128 个字符代表了计算中所有可能的文本元素。表示每个字符只需要七位（2⁷ = 128），因此字节和字符的概念是可以互换的。

自从创建 Unicode（通用字符集）来表示世界上所有的书写系统（甚至表情符号）以来，一些字符可能需要多达四个字节。Unicode 标准定义了多种编码字符的方式，包括 UTF-8（使用 8 位的 Unicode 转换格式）。正如我所说，文件 *tests/inputs/one.txt* 以字符 Ó 开头，在 UTF-8 中该字符长度为两个字节。如果你希望 `head` 显示这个一个字符，你必须请求两个字节：

```
$ head -c 2 tests/inputs/one.txt
Ó
```

如果我要求 head 只选择此文件的第一个字节，我会得到字节值 195，这不是一个有效的 UTF-8 字符串。输出是一个特殊字符，表示将字符转换为 Unicode 时出现问题：

```
$ head -c 1 tests/inputs/one.txt
?
```

挑战程序预计会重现这种行为。这是一个具有挑战性的程序，但你应该能够使用 std::io、std::fs::File 和 std::io::BufReader 来弄清楚如何从每个文件中读取字节和行。我在 tests/cli.rs 中包含了一套完整的测试，你应该已将其复制到你的源树中。务必经常运行 cargo test 来检查你的进度。在查看我的解决方案之前，尽力通过所有测试。

## 解决方案

我真的很惊讶于编写这个程序时学到了多少。我原本以为这是一个相当简单的程序，结果证明非常具有挑战性。我想逐步带你了解我是如何得出解决方案的，从如何逐行读取文件开始。

## 逐行读取文件

开始时，我将修改第 3 章中用于读取文件行的一些代码：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                for line in file.lines().take(config.lines)
{ ①
println!(" {}", line?); ②
    }
  }
    }
}
Ok(())
}
```

- ① 从文件句柄中获取所需数量的行。
- ② 将行打印到控制台。

我认为这是一个非常有趣的解决方案，因为它使用 `Iterator::take` 方法从 `config.lines` 中选择行数。我可以运行程序从包含三行的文件中选择一行，看起来效果很好：

```
$ cargo run -- -n 1 tests/inputs/three.txt
Three
```

如果我运行 `cargo test`，程序将通过几个测试，对于只实现了规范的一小部分来说，这已经相当不错了。它在所有以 *three* 开头的测试中都失败了，这些测试使用了 Windows 编码的输入文件。为了修复这个问题，我有一个 confession 要说。

## 读取文件时保留行尾

亲爱的读者，告诉你这件事让我很痛苦，但在第 3 章中我骗了你。我展示的 `cat` 程序并没有完全复制原始程序，因为它使用 `BufRead::lines` 来读取输入文件。该函数的文档说“返回的每个字符串末尾都不会有换行字节（0xA 字节）或 CRLF（0xD、0xA 字节）。”我希望你能原谅我，因为我想向你展示读取文件行的容易程度，但你

应注意 `cat` 程序会将 Windows CRLF 行结束符替换为 Unix 风格的新行。

要修复这个问题，我必须改用 `BufRead::read_line`，它说“此函数将从底层流中读取字节，直到找到换行符分隔符（`0xA` 字节）或 EOF¹。找到后，所有字节（包括分隔符（如果找到））都将追加到 `buf` 中。” 以下是会保留原始行结束符的版本。进行这些更改后，程序通过的测试会比失败的多：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match File::open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(mut file) => { ①
                let mut line = String::new(); ②
                for _ in 0..config.lines { ③
                    let bytes = file.read_line(&mut line)?;
                    if bytes == 0 { ⑤
                        break;
                    }
                    print!("{}", line); ⑥
                    line.clear(); ⑦
                }
            }
        };
    }
    Ok(())
}
```

- ① 将文件句柄作为 `mut`（可变）值接受。
- ② 使用 `String::new` 创建一个新的、空的可变字符串缓冲区来保存每一行。
- ③ 使用 `for` 遍历 `std::ops::Range`，从 0 计数到请求的行数。变量名 _ 表示我不打算使用它。
- ④ 使用 `BufRead::read_line` 读取下一行。
- ⑤ 文件句柄在到达末尾时会返回 0 字节，因此 `break` 跳出循环。
- ⑥ 打印包含原始行结束符的行。
- ⑦ 使用 `String::clear` 清空行缓冲区。

如果我此时运行 `cargo test`，读取行的测试几乎全部通过，而读取字节和处理多个文件的测试全部失败。

## 从文件读取字节

接下来，我将处理从文件读取字节。在尝试打开文件后，我会检查 `config.bytes` 是否为 `Some` 字节数；否则，我将使用前面读取行的代码：

```rust
for filename in config.files {
    match File::open(&filename) {
        Err(err) => eprintln!("{}: {}", filename, err),
        Ok(mut file) => {
            if let Some(num_bytes) = config.bytes { ①
                let mut handle = file.take(num_bytes as u64); ②
                let mut buffer = vec![0; num_bytes]; ③
                let n = handle.read(&mut buffer)?; ④
                print!("{}",
String::from_utf8_lossy(&buffer[..n])); ⑤
            } else {
                ... // Read lines as before
            }
        }
    };
}
```

- ① 使用模式匹配检查 `config.bytes` 是否为要读取的 `Some` 字节数。
- ② 使用 `take` 读取请求的字节数。
- ③ 创建一个长度固定为 `num_bytes`、填充零的可变缓冲区来保存从文件中读取的字节。
- ④ 从文件句柄读取所需数量的字节到缓冲区中。值 `n` 将报告实际读取的字节数，可能少于请求的数量。
- ⑤ 将字节转换为可能不是有效 UTF-8 的字符串。注意范围操作以仅选择实际读取的字节。

> **提示**
> 
> `std::io::Read` trait 中的 `take` 方法期望其参数为类型 `u64`，但我有一个 `usize`。我使用 `as` 关键字 `cast` 或转换该值。

这也许是我程序中最困难的部分。一旦我弄清楚如何只读取几个字节，我就必须弄清楚如何将它们转换为文本。如果我只取多字节字符的一部分，结果会失败，因为 Rust 中的字符串必须是有效的 UTF-8。我很高兴找到了 `String::from_utf8_lossy`，它会静默地将无效的 UTF-8 序列转换为 *未知* 或 *替换* 字符：

```
$ cargo run -- -c 1 tests/inputs/one.txt
```

让我展示我第一次尝试从文件读取字节的方法。我决定将整个文件读入一个字符串，将其转换为字节向量，并使用切片选择前 `num_bytes` 个字节。

```rust
let mut contents = String::new(); ①
file.read_to_string(&mut contents)?; // Danger here ②
let bytes = contents.as_bytes(); ③
print!("{}", String::from_utf8_lossy(&bytes[..num_bytes]));
// More danger ④
```

- ① 创建一个新的字符串缓冲区来保存文件的内容。
- ② 将整个文件内容读入字符串缓冲区。
- ③ 使用 `str::as_bytes` 将内容转换为字节（u8 或无符号 8 位整数）。
- ④ 使用 `String::from_utf8_lossy` 将字节切片转换为字符串。

## 警告

我向你展示这种方法是为了让你知道如何将文件读入字符串；然而，如果文件大小超过机器上的内存量，这可能非常危险。通常，除非你确信文件很小，否则这是一个糟糕的主意。

前述代码的另一个严重问题是它假设切片操作 `bytes[..num_bytes]` 会成功。例如，如果你用空文件使用此代码，你将请求不存在的字节。这将导致你的程序 `panic` 并立即退出并显示错误消息：

```
$ cargo run -- -c 1 tests/inputs/empty.txt
thread 'main' panicked at 'range end index 1 out of range for slice of
length 0', src/lib.rs:80:50
note: run with `RUST_BACKTRACE=1` environment variable to
display a backtrace
```

> **提示**
> 
> Rust 可以阻止你犯各种各样的严重错误，但它无法阻止你做愚蠢的事情。仍然有许多方法可以让你自己射中脚。

以下是也许从文件中读取所需字节数的最简短方法：

```rust
let bytes: Result<Vec<_, _>> =
file.bytes().take(num_bytes).collect();
print!("{}", String::from_utf8_lossy(&bytes?));
```

在前面的代码中，类型注解 `Result<Vec<_, _>>` 是必要的，因为编译器推断 `bytes` 的类型为切片，其大小未知。我必须指出我想要一个 `Vec`，它是指向堆分配内存的智能指针。这里的下划线（`_`）表示部分类型注解，基本上指示编译器推断类型。如果没有它，编译器会这样抱怨：

```
Compiling headr v0.1.0 (/Users/kyclark/work/sysprog-rust/playground/headr)
error[E0277]: the size for values of type `[u8]` cannot be known at compilation time
  --> src/lib.rs:95:58
   |
95 |                     print!("{}",
   | String::from_utf8_lossy(&bytes?);
   | ^^^^^^^ doesn't
   |                     have a size
known at compile-time
   |
   = help: the trait `Sized` is not implemented for `[u8]`
   = note: all local variables must have a statically known size
   = help: unsized locals are gated as an unstable feature
```

## 注意

你现在已经看到下划线 _ 有各种不同的功能。作为变量的前缀或名称，它告诉编译器你不想使用该值。在匹配臂中，它是处理任何情况的通配符。当在类型注解中使用时，它告诉编译器推断类型。

你也可以使用 turbofish 运算符（ ::<> ）在表达式的右侧指示类型信息。通常这只是风格问题，是否在左侧或右侧指示类型，但稍后你会看到 turbofish 对某些表达式是必需的示例：

```rust
let bytes = file.bytes().take(num_bytes).collect::<
<Result<Vec<_>, _>>();
```

`String::from_utf8_lossy` 产生的未知字符（b'\xef\xbf\xbd'）与 BSD head 产生的输出（b'\xc3'）并不完全相同，这使得测试变得有些困难。如果你查看 tests/cli.rs 中的 run 辅助函数，你会看到我读取期望值（head 的输出）并使用相同的函数转换可能无效的 UTF-8，以便我可以比较两个输出。run_stdin 函数的工作方式类似：

```rust
fn run(args: &[&str], expected_file: &str) -> TestResult {
    // Extra work here due to lossy UTF
    let mut file = File::open(expected_file)?;
    let mut buffer = Vec::new();
    file.read_to_end(&mut buffer)?;
    let expected = String::from_utf8_lossy(&buffer); ①

    Command::cargo_bin(PRG)?
        .args(args)
        .assert()
        .success()
        .stdout(predicate::eq(&expected.as_bytes() as &[u8])); ②
```



```
Ok(())
}
```

- 1. 处理 expected_file 中的任何无效 UTF-8。
- 2. 将输出和期望值作为字节切片 ([u8]) 进行比较。

## 打印文件分隔符

需要处理的最后一部分是多个文件之间的分隔符。如前所述，有效的文件都有一个头部，将文件名放在 ==> 和 <== 标记内。第一个文件之后的文件在开头有一个额外的换行符，以便在视觉上分隔输出。这意味着我需要知道正在处理的文件编号，我可以通过使用 Iterator::enumerate 方法获得。以下是将通过所有测试的 run 函数的最终版本：

```
pub fn run(config: Config) -> MyResult<()> {
    let num_files = config.files.len(); ①

    for (file_num, filename) in
config.files.iter().enumerate() { ②
        match File::open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                if num_files > 1 { ③
                    println!(
                        "{}==> {} <==",
                        if file_num > 0 { "\n" } else { "" }
                        filename
                    );
                }

                if let Some(num_bytes) = config.bytes {
                    let mut handle = file.take(num_bytes as
u64);
                    let mut buffer = vec![0; num_bytes];
let n = handle.read(&mut buffer)?;
print!("{}",
String::from_utf8_lossy(&buffer[..n]));
} else {
let mut line = String::new();
for _ in 0..config.lines {
let bytes = file.read_line(&mut
line)?;

if bytes == 0 {
break;
}
print!("{}", line);
line.clear();
}
}
}
};
}
Ok(())
}
```

- 1. 使用 `Vec::len` 方法获取文件数量。
- 2. 使用 `Iterator::enumerate` 方法同时跟踪文件编号和文件名。
- 3. 仅在存在多个文件时打印头部。
- 4. 打印换行符当 `file_num` 大于 0 时，这表示第一个文件。

## 更进一步

- 实现 GNU 版本的乘数后缀，例如 -c=1K 表示打印文件的前 1024 字节。务必添加并运行测试。

- 实现 GNU 版本的负数选项，其中 -n==-3 表示应打印除最后三行之外的所有内容。像往常一样，创建测试以确保程序正确。
- 添加选择字符的选项。
- 将带有 Windows 行结束符的文件添加到第 3 章的测试中。编辑该程序的 `mk-outs.sh` 以纳入此文件，然后扩展测试和程序以确保行结束符得以保留。

## 总结

本章深入探讨了一些相当棘手的话题，例如将类型如 `&str` 转换为 `usize`、`String` 转换为 `Error`，以及 `usize` 转换为 `u64`。我觉得理解 `&str` 和 `String` 之间的差异以及为什么需要使用 `From::from` 来创建 `MyResult` 的 `Err` 部分花了我相当长的时间。如果你仍然感到困惑，请知道你不会一直如此。我认为如果你继续阅读文档并编写更多代码，最终会明白的。

本练习中你完成了以下事项：

- 你学会了创建可以接受值的可选参数。之前，选项是标志。
- 你了解到所有命令行参数都是字符串。你使用了 `str::parse` 方法尝试将像“3”这样的字符串转换为数字 3。
- 你学会了如何为单个函数编写和运行单元测试。
- 你学会了使用 `as` 关键字或 `From` 和 `Into` 等 trait 进行类型转换。

- 你发现 _ 作为值的名称或前缀是一种向编译器表明你不打算使用该变量的方式。当在类型注解中使用时，它告诉编译器推断类型。
- 你了解到 `match` 分支可以包含一个称为守卫的额外布尔条件。
- 你学会了如何使用 `BufRead::read_line` 在读取文件句柄时保留行结束符。
- 你发现 `take` 方法同时适用于迭代器和文件句柄，用于限制所选元素的数量。
- 你学会了使用 turbofish 在赋值语句的左侧或右侧指示类型信息。
