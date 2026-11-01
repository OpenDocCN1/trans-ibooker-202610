
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
