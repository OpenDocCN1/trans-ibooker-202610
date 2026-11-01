
通常，位置参数是文件，并且包含像我的 $HOME 目录这样的目录会导致 grep 打印警告：

```
$ grep The bustle.txt $HOME fox.txt
bustle.txt:The bustle in a house
bustle.txt:The morning after death
bustle.txt:The sweeping up the heart,
grep: /Users/kyclark: Is a directory
fox.txt:The quick brown fox jumps over the lazy dog.
```

目录名只有在使用 -r|--recursive 选项查找包含匹配文本的目录中的所有文件时才可接受：

```
$ grep -r The .
./nobody.txt:Then there's a pair of us!
./bustle.txt:The bustle in a house
./bustle.txt:The morning after death
./bustle.txt:The sweeping up the heart,
./fox.txt:The quick brown fox jumps over the lazy dog.
```

-r 和 -i 短标志可以组合起来对一个或多个目录执行递归、不区分大小写的搜索：

```
$ grep -ri the .
./nobody.txt:Then there's a pair of us!
./nobody.txt:Don't tell! they'd advertise-you know!
./nobody.txt:To tell one's name-the livelong June-
./bustle.txt:The bustle in a house
./bustle.txt:The morning after death
./bustle.txt:The sweeping up the heart,
./fox.txt:The quick brown fox jumps over the lazy dog.
```

如果没有用于输入的位置参数，grep 将读取 STDIN：

```
$ cat * | grep -i the
The bustle in a house
The morning after death
The sweeping up the heart,
The quick brown fox jumps over the lazy dog.
Then there's a pair of us!
Don't tell! they'd advertise-you know!
To tell one's name-the livelong June-
```

这就是挑战程序预期要达到的范围。

## 开始

挑战程序的名称应为 `grepr`，用于 `grep` 的 Rust 版本。从 `cargo new grepr` 开始，然后将 *09_grepr/tests* 目录复制到你的新项目中。除了使用 `clap` 解析命令行参数外，我的解决方案将使用 `regex` 进行正则表达式，使用 `walkdir` 查找输入文件。以下是我的 *Cargo.toml* 的开始方式：

```
[dependencies]
clap = "2.33"
regex = "1"
walkdir = "2"
sys-info = "0.9"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

你可以运行 `cargo test` 执行初始构建并运行测试，所有测试都应该失败。

## 定义参数

我将为 *src/main.rs* 从以下内容开始：

```
fn main() {
    if let Err(e) = grepr::get_args().and_then(grepr::run)
{
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```

以下是我的 src/lib.rs 的开始方式。请注意，所有布尔选项默认为 false：

```
use clap::{App, Arg};
use regex::{Regex, RegexBuilder};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    pattern: Regex, ①
    files: Vec<String>, ②
    recursive: bool, ③
    count: bool, ④
    invert_match: bool, ⑤
}
```

① 模式是一个编译后的正则表达式。

② files 是一个字符串向量。

③ recursive 选项是一个布尔值，用于递归搜索目录。

④ count 选项是一个布尔值，用于显示匹配次数。

⑤ invert_match 是一个布尔值，用于查找不匹配模式的行。

以下是我的 get_args 和 run 函数的开始方式。你应该填入缺失的部分：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("grepr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust grep")
        // What goes here?
        .get_matches();

    Ok(Config {
        pattern: ...
        files: ...
        recursive: ...
        count: ...
        invert_match: ...
    })
}

pub fn run(config: Config) -> MyResult<()> {
    println!("{:#?}", config);
    Ok(())
}
```

你的程序应该能够产生以下用法：

```
grepr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust grep

USAGE:
    grepr [FLAGS] <PATTERN> <FILE>...

FLAGS:
    -c, --count          Count occurrences
    -h, --help           Prints help information
    -i, --insensitive    Case-insensitive
    -v, --invert-match   Invert match
    -r, --recursive      Recursive search
    -V, --version        Prints version information

ARGS:
    <PATTERN>    Search pattern ①
    <FILE>...    Input file(s) [default: -] ②
```

① 搜索模式是必需的参数。

② 输入文件是可选的，默认为 “-” 表示 STDIN。

当提供模式且没有输入文件时，你的程序应该能够打印如下的 Config：

```
$ cargo run -- dog
Config {
    pattern: dog,
    files: [
        "-",
    ],
    recursive: false,
    count: false,
    invert_match: false,
}
```

它应该能够处理一个或多个输入文件并处理标志：

```
$ cargo run -- dog -ricv tests/inputs/*.txt
Config {
    pattern: dog,
    files: [
        "tests/inputs/bustle.txt",
        "tests/inputs/empty.txt",
        "tests/inputs/fox.txt",
        "tests/inputs/nobody.txt",
    ],
    recursive: true,
    count: true,
    invert_match: true,
}
```

它应该拒绝无效的正则表达式。例如，* 表示前一个模式的零次或多次单独出现是不完整的：

```
$ cargo run -- \*
Invalid pattern "*"
```

我假设你已经弄明白了。以下是我的参数声明方式：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("grepr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust grep")
        .arg(
            Arg::with_name("pattern") ①
                .value_name("PATTERN")
                .help("Search pattern")
                .required(true),
        )
        .arg(
            Arg::with_name("files") ②
                .value_name("FILE")
                .help("Input file(s)")
                .required(true)
                .default_value("-")
                .min_values(1),
        )
        .arg(
            Arg::with_name("insensitive") ③
                .value_name("INSENSITIVE")
                .help("Case-insensitive")
                .short("i")
                .long("insensitive")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("recursive") ④
                .value_name("RECURSIVE")
                .help("Recursive search")
                .short("r")
                .long("recursive")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("count") ⑤
                .value_name("COUNT")
                .help("Count occurrences")
                .short("c")
                .long("count")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("invert") ⑥
                .value_name("INVERT")
                .help("Invert match")
                .short("v")
                .long("invert-match")
                .takes_value(false),
        )
        .get_matches();
```

- 1. 第一个位置参数用于 `pattern`。
- 2. 其余的位置参数用于输入。默认值为 "-"。
- 3. `insensitive` 标志将处理不区分大小写的选项。
- 4. `recursive` 标志将处理在目录中搜索文件。
- 5. `count` 标志将导致程序打印计数。
- 6. `invert` 标志将搜索不匹配模式的行。

**注意**

这里声明位置参数的顺序很重要，因为第一个定义的参数将用于第一个位置参数。你仍然可以在位置参数之前或之后定义选项。

接下来，我使用参数创建了一个正则表达式，该表达式将包含 `insensitive` 选项：

```
let pattern = matches.value_of("pattern").unwrap(); ①
let pattern = RegexBuilder::new(pattern) ②
    .case_insensitive(matches.is_present("insensitive")) ③
    .build() ④
    .map_err(|_| format!("Invalid pattern \"{}\"", pattern))?; ⑤

    Ok(Config { ⑥
        pattern,
        files: matches.values_of_lossy("files").unwrap(),
        recursive: matches.is_present("recursive"),
        count: matches.is_present("count"),
        invert_match: matches.is_present("invert"),
    })
}
```

- 1. `pattern` 是必需的，因此安全地解包值。
- 2. `RegexBuilder::new` 方法将创建一个新的正则表达式。
- 3. `Regex::case_insensitive` 方法将在存在 `insensitive` 标志时使正则表达式在比较时忽略大小写。
- 4. `Regex::build` 方法将编译正则表达式。
- 5. 如果 `build` 返回错误，使用 `Result::map_err` 创建一个错误消息，指出给定的模式无效。
- 6. 返回 `Config`。

RegexBuilder::build 将拒绝任何不是有效正则表达式的模式，这引出了一个有趣的点。编写正则表达式有许多语法。如果你仔细查看 grep 的手册页，你会注意到这些选项：

```
-E, --extended-regexp
    Interpret pattern as an extended regular expression
```
