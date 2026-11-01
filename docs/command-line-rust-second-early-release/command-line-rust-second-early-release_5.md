# 第5章 致母亲的话

### 早期发布读者的说明

使用早期发布的电子书，你可以获得书籍的最早形式——作者在写作时的原始未编辑内容——因此你可以在这些标题正式发布之前很久就利用这些技术。

这将是最终书籍的第 5 章。请注意，GitHub 仓库稍后才会激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者在本章中发现缺失的材料，请联系作者 kyClark@gmail.com。

> > 你可以用一只手数清我
> > —They Might Be Giants

久负盛名的 `wc` (*词数统计*) 程序可以追溯到 AT&T UNIX 的第 1 版。该程序将显示从 `STDIN` 或一个或多个文件中找到的文本的行数、词数和字节数。

在本练习中，你将学习：

- 如何使用 `Iterator::all` 函数
- 如何为测试创建模块
- 如何伪造文件句柄用于测试
- 如何有条件地格式化和打印值
- 如何将文本行拆分为单词和字符

- 如何使用 `Iterator::collect` 将迭代器转换为向量

### wc 如何工作

以下是 BSD wc 手册页的摘录，描述了该工具的工作原理：

```
WC(1)                                          BSD General Commands Manual
WC(1)

NAME
    wc -- word, line, character, and byte count

SYNOPSIS
    wc [-clmw] [file ...]

DESCRIPTION
    The wc utility displays the number of lines, words,
and bytes contained
    in each input file, or standard input (if no file is
specified) to the
    standard output.  A line is defined as a string of
characters delimited
    by a <newline> character.  Characters beyond the final
<newline> charac-
    ter will not be included in the line count.

    A word is defined as a string of characters delimited
by white space
    characters.  White space characters are the set of
characters for which
    the iswspace(3) function returns true.  If more than
one input file is
    specified, a line of cumulative counts for all the
files is displayed on
    a separate line after the output for the last file.

    The following options are available:

        -c      The number of bytes in each input file is
written to the standard
```

output. This will cancel out any prior usage
of the -m option.

-l      The number of lines in each input file is
written to the standard
            output.

-m      The number of characters in each input file is
written to the
            standard output.  If the current locale does
not support multi-
            byte characters, this is equivalent to the -c
option.  This will
            cancel out any prior usage of the -c option.

-w      The number of words in each input file is
written to the standard
            output.

When an option is specified, wc only reports the
information requested by
    that option.  The order of output always takes the
form of line, word,
    byte, and file name.  The default action is equivalent
to specifying the
    -c, -l and -w options.

If no files are specified, the standard input is used
and no file name is
    displayed.  The prompt will accept input until
receiving EOF, or [^D] in
    most environments.

一图胜千字，所以我将使用 05_wcr 目录中的测试文件展示一些示例。首先，考虑一个空文件，应报告 0 行、0 词和 0 字节，每个都应右对齐在宽 8 个字符的字段中：

```
$ cd 05_wcr
$ wc tests/inputs/empty.txt
      0       0       0 tests/inputs/empty.txt
```

接下来，尝试一个包含一行文本的文件。请注意，我在某些单词之间放置了不同数量的空格和一个制表符，原因将在后面讨论。我将使用带有 -t 标志的 cat 将制表符显示为 ^I，并使用 -e 将行尾显示为 $：

```
$ cat -te tests/inputs/fox.txt
The  quick brown fox^Ijumps over    the lazy dog.$
```

这个例子足够短，我可以手动计算所有行、词、字节，如图 5-1 所示，其中空格用 raised dots 表示，制表符用 \t 表示，行尾用 $ 表示。

![](img/fbe8ef8a7ecb95c985397f6370ec2c66_152_0.png)

*图 5-1. 有一行文本包含九个单词，共 48 字节*

我发现 wc 的结果一致：


```
$ wc tests/inputs/fox.txt
      1       9      48 tests/inputs/fox.txt
```

如第3章所述，*bytes* 对于 ASCII 来说可能等同于 *characters*，但 Unicode 字符可能占用多个字节。文件 *tests/inputs/atlamal.txt* 包含 *Atlamál hin groenlenzku* 或 *The Greenland Ballad of Atli* 的第一段诗句，这是一首古诺尔斯诗歌¹：

```
$ cat tests/inputs/atlamal.txt
Frétt hefir öld óvu, þá er endr of gerðu
seggir samkundu, sú varnýt fæstum,
æxtu einmæli, yggr var þeim síðan
ok it sama sonum Gjúka, er váru sannráðnir.
```

根据 wc，该文件包含 4 行、29 个词和 177 个字节：

```
$ wc tests/inputs/atlamal.txt
      4      29     177 tests/inputs/atlamal.txt
```

如果我只想要 *lines* 的数量，我可以使用 -l 标志，并且只会显示该列：

```
$ wc -l tests/inputs/atlamal.txt
      4 tests/inputs/atlamal.txt
```

我同样可以使用 -c 请求仅 *bytes* 的数量，或使用 -w 请求 *words*，并且只会显示这两个列：

```
$ wc -w -c tests/inputs/atlamal.txt
     29     177 tests/inputs/atlamal.txt
```

我可以使用 -m 标志请求 *characters* 的数量：

```
$ wc -m tests/inputs/atlamal.txt
    159 tests/inputs/atlamal.txt
```

GNU 版本的 wc 如果同时提供 -m 和 -c 标志会同时显示字符和字节计数，但 BSD 版本只会显示其中一个，后提供的标志优先：

```
$ wc -cm tests/inputs/atlamal.txt ①
    159 tests/inputs/atlamal.txt
$ wc -mc tests/inputs/atlamal.txt ②
    177 tests/inputs/atlamal.txt
```

① -m 标志在最后，所以显示的是字符。

② -c 标志在最后，所以显示的是字节。

请注意，无论标志的顺序如何，例如 -wc 或 -cw，输出列始终按 行、词、字节/字符 的顺序排列：

```
$ wc -cw tests/inputs/atlamal.txt
    29      177 tests/inputs/atlamal.txt
```

如果没有提供位置参数，wc 将从 STDIN 读取，并且不会打印文件名：

```
$ cat tests/inputs/atlamal.txt | wc -lc
    4       173
```

GNU 版本的 wc 会将文件名 - 理解为 STDIN，并且还提供长标志名以及其他一些选项：

```
$ wc --help
Usage: wc [OPTION]... [FILE]...
  or:  wc [OPTION]... --files0-from=F
Print newline, word, and byte counts for each FILE, and a
total line if
more than one FILE is specified.  With no FILE, or when
FILE is -,
read standard input.  A word is a non-zero-length sequence
```
由空白字符分隔的字符。
下面的选项可用于选择要打印的计数，始终按以下顺序：换行、词、字符、字节、最大行长度。

| 选项 | 描述 |
|---|---|
| -c, --bytes | 打印字节计数 |
| -m, --chars | 打印字符计数 |
| --files0-from=F | 从文件 F 中指定的以 NUL 结尾名称的文件读取输入；如果 F 为 - 则从标准输入读取名称 |
| -L, --max-line-length | 打印最长行的长度 |
| -w, --words | 打印词计数 |
| --help | 显示此帮助并退出 |
| --version | 输出版本信息并退出 |

如果处理多个文件，两个版本都会以 *total* 行结束，显示所有输入的行数、词数和字节数：

```
$ wc tests/inputs/*.txt
      4      29     177 tests/inputs/atlamal.txt
      0       0       0 tests/inputs/empty.txt
      1       9      48 tests/inputs/fox.txt
      5      38     225 total
```

不存在的文件会在处理过程中以警告形式记录到 STDERR：

```
$ wc tests/inputs/fox.txt blargh tests/inputs/atlamal.txt
      1       9      48 tests/inputs/fox.txt
wc: blargh: open: No such file or directory
      4      29     177 tests/inputs/atlamal.txt
      5      38     225 total
```

我还可以在 bash 中重定向文件句柄 2 来验证 wc 是否将警告打印到 STDERR：

```
$ wc tests/inputs/fox.txt blargh tests/inputs/atlamal.txt 2>err ①
      1       9      48 tests/inputs/fox.txt
      4      29     177 tests/inputs/atlamal.txt
      5      38     225 total
$ cat err ②
wc: blargh: open: No such file or directory
```

① 将输出句柄 2（STDERR）重定向到文件 err。

② 验证错误消息是否在文件中。

前述行为是挑战程序预期实现的大致内容。有一套全面的测试套件来帮助你逐步完成。

## 开始

挑战程序应命名为 wcr（读作 wick-er），作为我们 Rust 版本的 wc。使用 cargo new wcr 开始，然后修改你的 Cargo.toml 以添加以下依赖项：

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

将我的 05_wcr/tests 目录复制到你的新项目中并运行 cargo test 以执行初始构建并运行测试，所有测试都应该失败。我有时有点缺乏想象力，所以我将继续使用我在之前程序中使用的相同 src/main.rs 结构：

```
```rust
fn main() {
    if let Err(e) = wcr::get_args().and_then(wcr::run) {
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```
```

以下是你可以复制的 *src/lib.rs* 骨架。首先，这里是我定义 `Config` 以表示命令行参数的方式：

```
```rust
use clap::{App, Arg};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    files: Vec<String>, ①
    lines: bool, ②
    words: bool, ③
    bytes: bool, ④
    chars: bool, ⑤
}
```
```

① `files` 将是一个字符串向量。

② `lines` 是一个布尔值，用于指示是否打印行计数。

③ `words` 是一个布尔值，用于指示是否打印词计数。

④ `bytes` 是一个布尔值，用于指示是否打印字节计数。

⑤ `chars` 是一个布尔值，用于指示是否打印字符计数。

以下是你入门所需的两个函数。我会让你根据此骨架填充 `get_args`：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("wcr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust wc")
        // What goes here?
        .get_matches()

    Ok(Config {
        files: ...
        lines: ...
        words: ...
        bytes: ...
        chars: ...
    })
}
```

我建议你先通过打印配置来开始运行：

```
pub fn run(config: Config) -> MyResult<()> {
    println!("{:#?}", config);
    Ok(())
}
```

尝试让你的程序生成如下的 --help 输出：

```
$ cargo run -- --help
wcr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust wc

USAGE:
    wcr [FLAGS]  [FILE]...

FLAGS:
    -c, --bytes      Show byte count
    -m, --chars      Show character count
    -h, --help       Prints help information
    -l, --lines      Show line count
    -V, --version    Prints version information
    -w, --words      Show word count
```

```
ARGS:
    <FILE>...    Input file(s) [default: -]
```

我有点担心是否要模仿 BSD 或 GNU 版本的 wc 来组合 -m（字符）和 -c（字节）标志。我决定使用 BSD 行为，因此你的程序应禁止同时使用这两个标志：

```
$ cargo run -- -cm tests/inputs/fox.txt
error: The argument '--bytes' cannot be used with '--chars'
```

```
USAGE:
    wcr --bytes --chars
```

默认行为将是打印行、词和字节，这意味着当用户未明确请求时，配置中的这些值应为 true。确保你的程序会打印如下内容：

```
$ cargo run -- tests/inputs/fox.txt
Config {
    files: [ ①
        "tests/inputs/fox.txt",
    ],
    lines: true,
    words: true,
    bytes: true,
    chars: false, ②
}
```

+   ① 位置参数应出现在 files 中。
+   ② 除非存在 -m|--chars 标志，否则 chars 值应为 false。

如果存在任何单个标志，则未提及的所有其他标志应为 false：


```
$ cargo run -- -l tests/inputs/*.txt ①
Config {
    files: [
        "tests/inputs/atlamal.txt",
        "tests/inputs/empty.txt",
        "tests/inputs/fox.txt",
    ],
    lines: true, ②
    words: false,
    bytes: false,
    chars: false,
}
```

① -l 标志表示只想要行数统计。

② lines 值是唯一为 true 的值。

在此停止并让它工作到这个程度。我的狗需要洗澡，所以我马上回来。

我想你已经弄明白了，所以下面是 get_args 的第一部分。我声明参数的方式没有什么新东西，所以我不会对此进行评论：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("wcr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust wc")
        .arg(
            Arg::with_name("files")
                .value_name("FILE")
                .help("Input file(s)")
                .default_value("-")
                .min_values(1),
        )
        .arg(
            Arg::with_name("lines")
                .value_name("LINES")
                .help("Show line count")
                .takes_value(false)
.short("l")
.long("lines"),
)
.arg(
    Arg::with_name("words")
        .value_name("WORDS")
        .help("Show word count")
        .takes_value(false)
        .short("w")
        .long("words"),
)
.arg(
    Arg::with_name("bytes")
        .value_name("BYTES")
        .help("Show byte count")
        .takes_value(false)
        .short("c")
        .long("bytes"),
)
.arg(
    Arg::with_name("chars")
        .value_name("CHARS")
        .help("Show character count")
        .takes_value(false)
        .short("m")
        .long("chars")
        .conflicts_with("bytes"),
)
.get_matches();
```

在 clap 解析参数之后，我解包它们并尝试确定默认值：

```
let mut lines = matches.is_present("lines"); ①
let mut words = matches.is_present("words");
let mut bytes = matches.is_present("bytes");
let mut chars = matches.is_present("chars");

    if [lines, words, bytes, chars].iter().all(|v| v ==
&false) { ②
        lines = true;
        words = true;
        bytes = true;
        chars = false;
}

Ok(Config { ③
    files: matches.values_of_lossy("files").unwrap(),
    lines,
    words,
    bytes,
    chars,
})
}
```

- 1. 解包所有标志。
- 2. 如果所有标志都为 false，则将 `lines`、`words` 和 `bytes` 设置为 true。
- 3. 使用结构体字段初始化简写来设置值。

我想强调的是，我创建了一个包含所有标志的切片并调用 `slice::iter` 方法来创建迭代器。这样我就可以使用 `Iterator::all` 函数来判断所有值是否都为 false。此方法期望一个闭包，即可以作为参数传递给另一个函数的匿名函数。在这里，闭包是一个谓词或测试，用于判断某个元素是否为 false。我想知道所有标志是否都为 false，因此每个标志的引用都会作为参数传递给闭包。值是引用，所以我必须将每个值与 `&false` 进行比较，`&false` 是一个布尔值的引用。如果所有求值结果都为 true，则 `Iterator::all` 将返回 true²。

## 接受闭包的迭代器方法

你应该花一些时间阅读 `Iterator` 文档，注意其他接受闭包作为参数来选择、测试或转换元素的方法，包括：

- - `Iterator::any` 将在闭包对某项的求值即使只有一次返回 `true` 时返回 `true`。
- - `Iterator::filter` 将找到谓词为 `true` 的所有元素。
- - `Iterator::map` 将对每个元素应用闭包并返回一个包含转换后元素的 `std::iter::Map`。
- - `Iterator::find` 将返回满足谓词的迭代器的第一个元素作为 `Some(value)`，如果所有元素求值为 `false` 则返回 `None`。
- - `Iterator::position` 将返回满足谓词的第一个元素的索引作为 `Some(value)`，如果所有元素求值为 `false` 则返回 `None`。
- - `Iterator::cmp`、`Iterator::min_by` 和 `Iterator::max_by` 具有接受成对项目进行比较的谓词，分别用于找到最小值和最大值。

## 迭代文件

现在开始处理计数。你可以先处理每个文件，统计各种内容并打印所需的列。我建议你再次使用第 2 章中的 `open` 函数来打开文件：

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

请确保将你的导入扩展为以下内容：

```
use clap::{App, Arg};
use std::error::Error;
use std::fs::File;
use std::io::{self, BufRead, BufReader};
```

这里有一个 run 函数来帮你开始：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in &config.files {
        match open(filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(_file) => println!("Opened {}", filename),
        }
    }

    Ok(())
}
```

使用你的打开文件句柄，先尽可能简单地使用空文件，并确保你的程序为行、词和字节的三个列打印零：

```
$ cargo run -- tests/inputs/empty.txt
      0       0       0 tests/inputs/empty.txt
```

接下来使用 tests/inputs/fox.txt 并确保你得到以下计数。我特意添加了各种种类和数量的空白字符来挑战你如何将文本拆分为单词：

```
$ cargo run -- tests/inputs/fox.txt
    1       9       48 tests/inputs/fox.txt
```

请确保你的程序能正确处理 *tests/inputs/atlamal.txt* 中的 Unicode：

```
$ cargo run -- tests/inputs/atlamal.txt
    4       29      177 tests/inputs/atlamal.txt
```

并且你能正确统计字符数：

```
$ cargo run -- tests/inputs/atlamal.txt -wml
    4       29      159 tests/inputs/atlamal.txt
```

当你能处理任意一个文件时，使用多个文件来检查你是否打印了正确的 *total* 列：

```
$ cargo run -- tests/inputs/*.txt
    4       29      177 tests/inputs/atlamal.txt
    0       0       0 tests/inputs/empty.txt
    1       9       48 tests/inputs/fox.txt
    5       38      225 total
```

当所有这些都能正常工作时，尝试从 STDIN 读取：

```
$ cat tests/inputs/atlamal.txt | cargo run
    4       29      177
```

经常运行 `cargo test` 来查看你的进展。在通过所有测试之前不要阅读后续内容。

## Solution

我想带你了解一下我是如何得出解决方案的，我想强调的是，我的方案只是众多可能写法中的一种。只要你的代码通过测试并且产生的输出与 BSD 版本的 wc 相同，那么它就很好，你应该为自己的成就感到自豪。

## 统计文件或 STDIN 的元素

我建议你先迭代文件名并打印文件的计数。为此，我决定创建一个名为 count 的函数，它将接受一个文件句柄并可能返回一个名为 FileInfo 的结构体，其中包含行数、单词数、字节数和字符数，每个都表示为 usize。我说该函数可能返回此结构体，因为该函数涉及 IO，而 IO 可能会出错。我将以下定义放在 Config 结构体之后。出于我稍后会解释的原因，它除了 Debug 之外还必须派生 PartialEq 特性：

```
#[derive(Debug, PartialEq)]
pub struct FileInfo {
    num_lines: usize,
    num_words: usize,
    num_bytes: usize,
    num_chars: usize,
}
```

为了表示该函数可能成功或失败，它将返回一个 MyResult<FileInfo>，这意味着成功时为 Ok<FileInfo>，失败时为 Err。为了开始这个函数，我将初始化一些可变变量来计数所有元素并返回一个 FileInfo 结构体：

```
pub fn count(mut file: impl BufRead) -> MyResult<FileInfo>
{ ①
    let mut num_lines = 0; ②
    let mut num_words = 0;
    let mut num_bytes = 0;
    let mut num_chars = 0;

    Ok(FileInfo {
        num_lines, ③
```



```
num_words,
num_bytes,
num_chars,
})
}
```

- 1. `count` 函数将接受一个可变的 `file` 值，并且可能会返回一个 `FileInfo` 结构体。
- 2. 初始化可变变量以统计行数、单词数、字节数和字符数。
- 3. 目前，返回一个所有值为零的 `FileInfo`。

## 注
我正在引入 `impl` 关键字来表示 `file` 值必须实现 `BufRead` trait。请回忆 `open` 返回的值满足此条件。你很快就会看到这如何使函数更灵活。

在第 3 章中，我展示了如何编写单元测试，将其放置在正在测试的函数之后。我将为 `count` 函数创建一个单元测试，但这次我将把它放在名为 `tests` 的模块中。这是一种整洁的分组单元测试方式，我可以使用一个配置选项告诉 Rust 仅在测试期间编译该模块。这尤其有用，因为我想在测试中使用 `std::io::Cursor` 来为 `count` 函数制作一个假文件句柄。当不进行测试时构建和运行程序时，该模块不会被包含。

`Cursor` 是“用于内存缓冲区，任何实现 `AsRef<[u8]>` 的内容，以使它们实现 `Read` 和/或 `Write`，从而允许这些缓冲区在任何可能使用实际执行 I/O 的读取器或写入器的地方使用。” 以下是创建 `tests` 模块然后导入并测试 `count` 函数的方式：

```
#[cfg(test)] ①
mod tests { ②
    use super::{count, FileInfo}; ③
    use std::io::Cursor; ④

    #[test]
    fn test_count() {
        let text = "I don't want the world. I just want your half.\r\n";
        let info = count(Cursor::new(text)); ⑤
        assert!(info.is_ok()); ⑥
        let expected = FileInfo {
            num_lines: 1,
            num_words: 10,
            num_chars: 48,
            num_bytes: 48,
        };
        assert_eq!(info.unwrap(), expected); ⑦
    }
}
```

- 1. `cfg` 启用条件编译，因此此模块仅在测试时编译。
- 2. 定义一个名为 `tests` 的新模块 (`mod`) 来包含测试代码。
- 3. 从父模块 `super` 导入 `count` 函数和 `FileInfo` 结构体，其含义是 *在其上方或更高*，指的是包含 `tests` 的上层模块。
- 4. 导入 `std::io::Cursor`。
- 5. 使用实现 `BufRead` 的 `Cursor` 运行 `count`。
- 6. 确保结果为 `Ok`。
- 7. 将结果与预期值进行比较。此比较要求 `FileInfo` 派生 `PartialEq`。

使用 **cargo test test_count** 运行此测试。你会看到 Rust 编译器关于未使用变量或不需要可变的变量的许多警告。最重要的结果是测试失败：

```
failures:

---- tests::test_count stdout ----
thread 'tests::test_count' panicked at 'assertion failed:
`(left == right)`
    left: `FileInfo { num_lines: 0, num_words: 0, num_bytes:
0, num_chars: 0 }`,
    right: `FileInfo { num_lines: 1, num_words: 10, num_bytes:
48,
    num_chars: 48 }`', src/lib.rs:146:9
```

花点时间编写 `count` 函数的其余部分，使其通过此测试。我会带我的爱干净的小狗去散步，也许喝点茶。

OK，我们回来了。现在我将展示我如何编写我的 `count` 函数。我知道从第 3 章中 `BufRead::lines` 会删除行结束符，而我不想要它，因为 Windows 文件中的换行是两个字节（\r\n），而 Unix 换行只有一个字节（\n）。我可以复制第 3 章中使用 `BufRead::read_line` 代替的代码来将每一行读入缓冲区。方便的是，此函数告诉我已从文件中读取了多少字节：

```
pub fn count(mut file: impl BufRead) -> MyResult<FileInfo>
{
    let mut num_lines = 0;
    let mut num_words = 0;
    let mut num_bytes = 0;
    let mut num_chars = 0;
    let mut line = String::new(); ①

    loop { ②
        let line_bytes = file.read_line(&mut line)?; ③
        if line_bytes == 0 { ④
            break;
        }
        num_bytes += line_bytes; ⑤
        num_lines += 1; ⑥
        num_words += line.split_whitespace().count(); ⑦
        num_chars += line.chars().count(); ⑧
        line.clear(); ⑨
    }

    Ok(FileInfo {
        num_lines,
        num_words,
        num_bytes,
        num_chars,
    })
}
```

- ① 创建一个可变缓冲区来保存每行 `line` 文本。
- ② 创建一个无限 `loop` 用于读取文件句柄。
- ③ 尝试从文件句柄读取一行。
- ④ 当读取零字节时表示已到达文件末尾（EOF），因此 `break` 退出循环。
- ⑤ 将此行的字节数加到 `num_bytes` 变量。
- ⑥ 每次循环都是一行，因此递增 `num_lines`。
- ⑦ 使用 `str::split_whitespace` 方法按空白分隔字符串，并使用 `Iterator::count` 找到单词数。
- ⑧ 使用 `str::chars` 方法将字符串拆分为 Unicode 字符，并使用 `Iterator::count` 计数字符。
- ⑨ 清空 `line` 缓冲区以供下一行文本使用。

## 注
我之前强调过此输入文件在分隔单词时具有不同数量和类型的空白。我这样做是为了以防你选择迭代字符来查找单词边界。也就是说，你可能没有找到 `str::split_whitespace`，而是使用了 `str::chars` 的迭代器。如果你每次找到空白时就递增单词数，假设单词之间只有一个空白，你可能会过度计数单词。相反，你需要将空白的连续运行作为单词之间的分隔符。同样，你可能想过使用正则表达式，这也需要考虑一个或多个空白字符。

有了这些更改，`test_count` 通过了。我想看看它看起来如何，所以我可以更改 `run` 以打印成功计数文件元素的結果，或在文件无法打开时向 `STDERR` 打印警告：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in &config.files {
        match open(filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                if let Ok(info) = count(file) { ①
                    println!("{:?}", info); ②
                }
            }
        }
    }
    Ok(())
}
```

- ① 尝试从文件中获取计数。
- ② 打印计数。

当我在其中一个测试输入上运行时，对于有效文件似乎可以工作：

```
$ cargo run -- tests/inputs/fox.txt
FileInfo { num_lines: 1, num_words: 9, num_bytes: 48,
num_chars: 48 }
```

它甚至可以处理从 STDIN 读取：

```
$ cat tests/inputs/fox.txt | cargo run
FileInfo { num_lines: 1, num_words: 9, num_bytes: 48,
num_chars: 48 }
```

现在要使输出符合规范。

## 格式化输出
要创建预期输出，我可以先更改 run 始终打印行数、单词数和字节数，然后跟上文件名：

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in &config.files {
        match open(filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                if let Ok(info) = count(file) {
                    println!(
                        "{:>8}{:>8}{:>8} {}", ①
                        info.num_lines,
                        info.num_words,
                        info.num_bytes,
                        filename
                    );
                }
            }
        }
    }
    Ok(())
}
```

① 将行数、单词数和字节数格式化为宽度为 8 字符的右对齐字段。

如果我用一个输入文件运行它，已经看起来很不错了：

```
$ cargo run -- tests/inputs/fox.txt
      1       9       48 tests/inputs/fox.txt
```

如果我运行 cargo test fox，我通过了八项测试中的一项。万岁！

```
running 8 tests
test fox ... ok
test fox_bytes ... FAILED
test fox_chars ... FAILED
test fox_bytes_lines ... FAILED
test fox_words_bytes ... FAILED
test fox_words ... FAILED
test fox_words_lines ... FAILED
test fox_lines ... FAILED
```

检查 tests/cli.rs 以查看通过的测试是什么样子的。请注意，测试引用了在模块顶部声明的常量值：

```
const PRG: &str = "wcr";
const EMPTY: &str = "tests/inputs/empty.txt";
const FOX: &str = "tests/inputs/fox.txt";
const ATLAMAL: &str = "tests/inputs/atlamal.txt";
```

我再次有一个 run 辅助函数来运行我的测试：

```
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

- 1. 尝试读取此命令的预期输出。



2 使用给定参数运行 wc 程序。断言程序成功并且 STDOUT 与预期值匹配。

fox 测试使用 FOX 输入文件且不带选项运行 wc，并将其与使用 05_wc/mk-outs.sh 生成的预期输出文件的内容进行比较：

```
#[test]
fn fox() -> TestResult {
    run(&[FOX], "tests/expected/fox.txt.out")
}
```

查看文件中的下一个函数以查看失败的测试：

```
#[test]
fn fox_bytes() -> TestResult {
    run(&["--bytes", FOX], "tests/expected/fox.txt.c.out")
}
```

1 使用相同的输入文件和 --bytes 选项运行 wc 程序。

使用 --bytes 运行 时，我的程序应该只打印该列的输出，但它总是打印行、词和字节。我决定在 src/lib.rs 中编写一个名为 format_field 的函数，该函数会根据布尔值有条件地返回格式化的字符串或空字符串：

```
fn format_field(value: usize, show: bool) -> String { ①
    if show { ②
        format!("{:>8}", value) ③
    } else {
        "".to_string() ④
    }
}
}
```

- 1. 该函数接受一个 `usize` 值和一个布尔值并返回一个 `String`。
- 2. 检查 `show` 值是否为 `true`。
- 3. 通过将数字格式化为宽 8 个字符的字符串来返回一个新字符串。
- 4. 否则，返回空字符串。

## NOTE

为什么此函数返回 `String` 而不是 `str`？它们都是 *字符串*，但 `str` 是一个不可变、固定长度的字符串。从函数返回的字符串是在运行时动态生成的，因此我必须使用 `String`，它是一个可增长的、堆分配的结构。感谢像 Perl 和 Python 这样的动态语言，它们隐藏了字符串的许多复杂性，而这些复杂性比我曾经考虑的要复杂得多。

我可以扩展我的 `tests` 模块以添加此功能的单元测试：

```
#[cfg(test)]
mod tests {
    use super::{count, format_field, FileInfo}; ①
    use std::io::Cursor;

    #[test]
    fn test_count() {} // Same as before

    #[test]
    fn test_format_field() {
    assert_eq!(format_field(1, false), ""); ②
    assert_eq!(format_field(3, true), "       3"); ③
    assert_eq!(format_field(10, true), "       10"); ④
}
```

- 1. 将 `format_field` 添加到导入中。
- 2. 当 `show` 为 `false` 时，函数应返回空字符串。
- 3. 检查单个数字的宽度。
- 4. 检查双位数字的宽度。

以下是我在上下文中使用它的方式，其中我还处理从 STDIN 读取时打印空字符串的情况：

```rust
pub fn run(config: Config) -> MyResult<()> {
    for filename in &config.files {
        match open(filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                if let Ok(info) = count(file) {
                    println!(
                        "{}{}{}{}{}", ①
                        format_field(info.num_lines,
                        config.lines),
                        format_field(info.num_words,
                        config.words),
                        format_field(info.num_bytes,
                        config.bytes),
                        format_field(info.num_chars,
                        config.chars),
                        if filename.as_str() == "-" { ②
                            "".to_string()
                        } else {
                            format!(" {}", filename)
                        }
                    );
                }
            }
        }
    }
}
```

```
Ok(())
}
```

- 1. 使用 `format_field` 函数格式化每个列的输出。
- 2. 当文件名是 "-" 时，打印空字符串；否则，打印一个空格和文件名。

有了这些更改，`cargo test fox` 的所有测试都通过了。如果我运行整个测试套件，我仍然会失败 *all* 测试：

```
failures:
    test_all
    test_all_bytes
    test_all_bytes_lines
    test_all_lines
    test_all_words
    test_all_words_bytes
    test_all_words_lines
```

查看 *tests/cli.rs* 中的 `all` 函数，可以看到测试使用所有输入文件作为参数：

```
#[test]
fn all() -> TestResult {
    run(&[EMPTY, FOX, ATLAMAL], "tests/expected/all.out")
}
```

如果我使用所有输入文件运行当前程序，我可以看到我缺少 *total* 行：

```
$ cargo run -- tests/inputs/*.txt
    4      29      177 tests/inputs/atlamal.txt
    0      0       0 tests/inputs/empty.txt
    1      9       48 tests/inputs/fox.txt
```

以下是我的最终 run 函数，它保留运行总计，并在输入多于一个时打印这些值：

```
pub fn run(config: Config) -> MyResult<()> {
    let mut total_lines = 0; ①
    let mut total_words = 0;
    let mut total_bytes = 0;
    let mut total_chars = 0;

    for filename in &config.files {
        match open(filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                if let Ok(info) = count(file) {
                    println!(
                        "{}{}{}{}",
                        format_field(info.num_lines,
                        config.lines),
                        format_field(info.num_words,
                        config.words),
                        format_field(info.num_bytes,
                        config.bytes),
                        format_field(info.num_chars,
                        config.chars),
                        if filename.as_str() == "-" {
                            "".to_string()
                        } else {
                            format!(" {}", filename)
                        }
                    );

                    total_lines += info.num_lines; ②
                    total_words += info.num_words;
                    total_bytes += info.num_bytes;
                    total_chars += info.num_chars;
                }
            }
        }
    }

    if config.files.len() > 1 { ③
        println!(
            "{}{}{}{} total",
            format_field(total_lines, config.lines),
```

```
format_field(total_words, config.words),
format_field(total_bytes, config.bytes),
format_field(total_chars, config.chars)
);
}
    Ok(())
}
```

- 1. 创建可变变量以跟踪行数、词数、字节数和字符数的总数。
- 2. 使用此文件的值更新总数。
- 3. 如果输入多于一个，则打印总数。

这似乎工作良好：

```
$ cargo run -- tests/inputs/*.txt
      4      29      177 tests/inputs/atlamal.txt
      0       0        0 tests/inputs/empty.txt
      1       9       48 tests/inputs/fox.txt
      5      38      225 total
cargo run -- -m
tests/inputs/atlamal.txt
```

我可以计算字符而不是字节：

```
$ cargo run -- -m tests/inputs/atlamal.txt
      159 tests/inputs/atlamal.txt
```

我可以显示和隐藏任何我想要的列：

```
$ cargo run -- -wc tests/inputs/atlamal.txt
      29      177 tests/inputs/atlamal.txt
```

最重要的是，cargo test 显示所有测试都通过。

## Going Further

编写一个模仿 GNU `wc` 而不是 BSD 版本的输出的版本。如果你的系统已经有 GNU 版本，运行 `mk-outs.sh` 程序为给定的输入文件生成预期的输出。修改程序以根据测试创建正确的输出。然后扩展程序以处理额外的选项，如 `--files0-from` 用于从文件中读取输入文件名和 `--max-line-length` 用于打印最长行的长度。为新功能添加测试。

接下来，思考本章开头提到的 BSD 手册页中提到的 `iswspace` 函数的奥秘。我曾想在原始日文字符³ 中包含第 2 章的 Issa 俳句的测试文件：

*隅の蜘蛛案じな煤はとらぬぞよ*

—Issa

BSD `wc` 认为有 3 个词：

```
$ wc spiders.txt
      1       3      40 spiders.txt
```

GNU 版本说只有一个词：

```
$ wc spiders.txt
  1  1 40 spiders.txt
```

我不想打开那个潘多拉魔盒，但如果你要创建一个发布给公众的程序版本，你会报告多少个词的数量？

## Summary

反思你在本章的进展：

-   你了解到 `Iterator::all` 函数会在所有元素对给定谓词求值为 `true` 时返回 `true`，该谓词是一个接受元素的闭包。许多类似的 `Iterator` 方法接受闭包作为参数，用于测试、选择和转换元素。
-   你使用了 `str::split_whitespace` 和 `str::chars` 方法将文本拆分为单词和字符。
-   你使用了 `Iterator::count` 方法来计数项目的数量。
-   你编写了一个函数，根据标志参数有条件地格式化值或空字符串，以支持信息的打印或省略。
-   你将单元测试组织到 `tests` 模块中，并从名为 `super` 的父模块导入函数。
-   你看到了如何使用 `std::io::Cursor` 创建一个假文件句柄，用于测试期望实现 `BufRead` 的函数。
-   在大约 200 行 Rust 代码中，你编写了一个相当不错的替代品，替代了最广泛使用的 Unix 程序之一。

1 There are many who know how of old did men, In counsel gather; little good did they get; In secret they plotted, it was sore for them later, And for Gjuki’s sons, whose trust they deceived.
许多人知道古时人们如何，在商议中聚集；收获甚少；他们暗中密谋，后来为此痛苦，并让格尤基之子蒙受欺骗，辜负了他们的信任。

2 When my youngest first started brushing his own teeth before bed, I would ask if he’d brushed and flossed. The problem was that he was prone to fibbing, so it was hard to trust him. In an actual exchange one night, I asked “Did you brush and floss your teeth?” Yes, he replied. “Did you brush your teeth?” Yes, he replied. “Did you floss your teeth?” No, he replied. So clearly he failed to properly combine Boolean values because a `true` statement and a `false` statement should result in a `false` outcome.
当我最小的孩子第一次开始在睡前自己刷牙时，我会问他是否刷牙并使用了牙线。问题是他容易撒谎，所以很难信任他。某天晚上的一次实际交流中，我问“你刷牙并用牙线了吗？”他回答“是”。“你刷牙了吗？”他回答“是”。“你用牙线了吗？”他回答“没有”。所以很明显他未能正确组合布尔值，因为一个 `true` 语句和一个 `false` 语句的结果应为 `false`。

3 A more literal translation might be “Corner spider, rest easy, my soot-broom is idle.”
更直译可能是“角落的蜘蛛，放心吧，我的煤灰扫帚闲着呢。”
