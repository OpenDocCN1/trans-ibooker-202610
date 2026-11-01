# 第10章. 波士顿公共绿地

### 早期发布读者的说明

使用 Early Release 电子书，你可以在作者撰写时获得书籍的最早形式——作者的原始未编辑内容——这样你就可以在这些标题正式发布之前很长时间内利用这些技术。

这将是最终书籍的第 10 章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者在本章中发现缺失的材料，请联系作者 kyle clark@gmail.com。

> *你能责怪这种常识吗？*
> 
> —They Might Be Giants

`comm`（*通用*）实用程序将读取两个文件并报告同时存在于两者中的文本行以及各自独有的行。这些是集合操作，其中共同的行是两个文件的 *交集*，而唯一的行是 *差集*。如果你熟悉数据库，你也可以将它们视为 *连接* 操作的类型。在本章中，你将学习：

- 如何手动迭代文件句柄的行
- 如何使用元组通过 `match` 匹配可能性组合
- 如何比较两个字符串
- 如何使用 `std::cmp::Ordering`
- 如何创建包含值的 `enum` 变体

### comm 如何工作

我将从 BSD `comm` 手册页的一部分开始：

```
COMM(1)                                      BSD General Commands Manual
COMM(1)

NAME
        comm -- select or reject lines common to two files

SYNOPSIS
        comm [-123i] file1 file2

DESCRIPTION
        The comm utility reads file1 and file2, which should
be sorted lexically,
        and produces three text columns as output: lines only
in file1; lines
        only in file2; and lines in both files.

        The filename '--' means the standard input.

        The following options are available:

        -1          Suppress printing of column 1.

        -2          Suppress printing of column 2.

        -3          Suppress printing of column 3.

        -i          Case insensitive comparison of lines.

        Each column will have a number of tab characters
prepended to it equal to
        the number of lower numbered columns that are being
printed.  For example, if column number two is being suppressed, lines
printed in column number one will not have any tabs preceding them, and
lines printed in column number three will have one.
```

comm 实用程序假定文件按词法排序；所有字符都参与行比较。

看一些示例可能会有所帮助：

### 示例

假设有一个名为 example.txt 的文件，其内容如下：

```
a
b
c
d
```

显示仅在 example.txt 中的行、仅在 stdin 中的行以及共同行：

```
$ echo -e "B\nc" | comm example.txt -
      B
a
b
      c
d
```

显示仅共同行，进行不区分大小写的比较：

```
$ echo -e "B\nc" | comm -1 -2 -i example.txt -
b
c
```

### GNU 版本有一些额外的选项：

```
$ comm --help
Usage: comm [OPTION]... FILE1 FILE2
Compare sorted files FILE1 and FILE2 line by line.

When FILE1 or FILE2 (not both) is -, read standard input.

With no options, produce three-column output. Column one contains
lines unique to FILE1, column two contains lines unique to FILE2,
and column three contains lines common to both files.

- -1 suppress column 1 (lines unique to FILE1)
- -2 suppress column 2 (lines unique to FILE2)
- -3 suppress column 3 (lines that appear in both files)
- --check-order check that the input is correctly sorted, even if all input lines are pairable
- --nocheck-order do not check that the input is correctly sorted
- --output-delimiter=STR separate columns with STR
- --total output a summary
- -z, --zero-terminated line delimiter is NUL, not newline
- --help display this help and exit
- --version output version information and exit

Note, comparisons honor the rules specified by 'LC_COLLATE'.

Examples:
  comm -12 file1 file2  Print only lines present in both file1 and file2.
  comm -3 file1 file2  Print lines in file1 not in file2, and vice versa.

At this point, you may be wondering exactly why you’d use this. Consider that you have a list of cities where your favorite band played on their last tour:

```
$ cd 10_commr/tests/inputs/
$ cat cities1.txt
Jackson
Denton
Cincinnati
Boston
Santa Fe
Tucson
```



另一个文件列出了他们当前巡演的城市：

```
$ cat cities2.txt
San Francisco
Denver
Ypsilanti
Denton
Cincinnati
Boston
```

你可以使用 comm 来查找同时出现在两个集合中的城市，通过抑制第 1 列（仅存在于第一个文件中的行）和第 2 列（仅存在于第二个文件中的行），只显示第 3 列（同时存在于两个文件中的行）。这类似于 SQL 中的内连接，其中只显示同时出现在两个输入中的数据。请注意，两个文件都需要先排序：

```
$ comm -12 <(sort cities1.txt) <(sort cities2.txt)
Boston
Cincinnati
Denton
```

如果你想查找他们仅在第一次巡演中演出的城市，可以抑制第 2 列和第 3 列：

```
$ comm -23 <(sort cities1.txt) <(sort cities2.txt)
Jackson
Santa Fe
Tucson
```

最后，如果你想查找他们仅在第二次巡演中演出的城市，可以抑制第 1 列和第 3 列：

```
$ comm -13 <(sort cities1.txt) <(sort cities2.txt)
Denver
San Francisco
Ypsilanti
```

第一个或第二个文件可以是 STDIN，用文件名“-”表示：

```
$ sort cities2.txt | comm -12 <(sort cities1.txt) -
Boston
Cincinnati
Denton
```

与 GNU comm 一样，在挑战程序中输入只能有一个是“-”。请注意，当存在 -i 标志时，comm 可以执行不区分大小写的比较。例如，我可以将第一次巡演的城市设为小写：

```
$ cat cities1_lower.txt
jackson
denton
cincinnati
boston
santa fe
tucson
```

而第二次巡演的城市设为大写：

```
$ cat cities2_upper.txt
SAN FRANCISCO
DENVER
YPSILANTI
DENTON
CINCINNATI
BOSTON
```

然后我可以使用 -i 标志来查找共同的城市：

```
$ comm -i -12 <(sort cities1_lower.txt) <(sort cities2_upper.txt)
boston
cincinnati
denton
```

## 注意

我知道巡演城市的例子很 trivial，所以我将给你另一个例子，来源于我在生物信息学方面的经验，生物信息学是计算机科学和生物学的交叉领域。给定一个蛋白质序列文件，我可以运行一个分析，将相似的序列分组到簇中。然后我可以使用 `comm` 将聚类的蛋白质与原始列表进行比较，找到未能聚类的蛋白质。这些未聚类的蛋白质可能具有某些独特之处，值得进一步分析。

这就是挑战程序预期实现的内容。与原始工具的一个变化是我想要添加一个可选的输出列分隔符，默认为制表符字符，这是 `comm` 的正常输出。

## 开始使用

本章中的程序将被称为 `commr`（读作 *comm-ar*），用于 `comm` 的 Rust 版本。我建议你使用 `cargo new commr` 开始，然后在你的 *Cargo.toml* 文件中添加以下依赖项：

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

将我的 *10_commr/tests* 目录复制到你的项目中，然后运行 `cargo test` 运行测试，这些测试应该全部失败。

## 定义参数

这里没有意外，但我建议你在 *src/main.rs* 中使用以下内容：

```
fn main() {
    if let Err(e) = commr::get_args().and_then(commr::run)
    {
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```

你可以从以下 Config 定义开始 src/lib.rs：

```
use clap::{App, Arg};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    file1: String, ①
    file2: String, ②
    suppress_col1: bool, ③
    suppress_col2: bool, ④
    suppress_col3: bool, ⑤
    insensitive: bool, ⑥
    delimiter: String, ⑦
}
```

+   1. 第一个输入文件名是 String。
+   2. 第二个输入文件名是 String。
+   3. 一个布尔值，用于是否抑制输出的第一列。
+   4. 一个布尔值，用于是否抑制输出的第二列。
+   5. 一个布尔值，用于是否抑制输出的第三列。
+   6. 一个布尔值，用于是否执行不区分大小写的比较。
+   7. 输出列分隔符，默认为制表符。

你可以这样开始你的 get_args：

```rust
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("commr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust comm")
        // What goes here?
        .get_matches();

    Ok(Config {
        file1: ...
        file2: ...
        suppress_col1: ...
        suppress_col2: ...
        suppress_col3: ...
        insensitive: ...
        delimiter: ...
    })
}
```

通过打印配置开始你的 run 函数：

```rust
pub fn run(config: Config) -> MyResult<()> {
    println!("{:#?}", config);
    Ok(())
}
```

你的程序应该能够产生以下用法：

```
cargo run -- -h
commr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust comm

USAGE:
    commr [FLAGS] [OPTIONS] <FILE1> <FILE2>

FLAGS:
    -h, --help      Prints help information
    -i              Case insensitive comparison of lines
    -1              Suppress printing of column 1
```

|  |  |
|---|---|
| -2 | 抑制打印第 2 列 |
| -3 | 抑制打印第 3 列 |
| -V, --version | 打印版本信息 |

### 选项：

-d, --output-delimiter <DELIM>    输出分隔符

### 参数：

<FILE1>    输入文件 1
<FILE2>    输入文件 2

如果你不带参数运行程序，它应该失败，并显示两个文件参数是必需的消息：

```
$ cargo run
error: The following required arguments were not provided:
  <FILE1>
  <FILE2>
```

### 用法：

```
commr [FLAGS] [OPTIONS] <FILE1> <FILE2>
```

有关更多信息请尝试 --help

如果你提供两个位置参数，你应该得到以下输出：

```
$ cargo run -- tests/inputs/file1.txt tests/inputs/file2.txt
Config {
  file1: "tests/inputs/file1.txt", ①
  file2: "tests/inputs/file2.txt",
  suppress_col1: false, ②
  suppress_col2: false,
  suppress_col3: false,
  insensitive: false,
  delimiter: "\t",
}
```

① 两个位置参数被解析为 file1 和 file2。

② 其余所有值使用默认值，布尔值为 false，分隔符为制表符。

验证你也可以设置其他所有参数：

```
$ cargo run -- tests/inputs/file1.txt tests/inputs/file2.txt -123 -d , -i
Config {
  file1: "tests/inputs/file1.txt",
  file2: "tests/inputs/file2.txt",
  suppress_col1: true, ①
  suppress_col2: true,
  suppress_col3: true,
  insensitive: true, ②
  delimiter: ",", ③
}
```

① -123 将每个 suppress 值设为 true。

② -i 将 insensitive 设为 true。

③ -d 选项将分隔符设为逗号（,）。

停止阅读并使你的程序匹配前面的输出。完成后回来。

以下是我在 get_args 中定义参数的方式。由于它与之前的程序非常相似，这里没什么好评论的：

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("commr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust comm")
        .arg(
            Arg::with_name("file1")
                .value_name("FILE1")
```

```
.help("Input file 1")
.takes_value(true)
.required(true),
)
.arg(
    Arg::with_name("file2")
        .value_name("FILE2")
        .help("Input file 2")
        .takes_value(true)
        .required(true),
)
.arg(
    Arg::with_name("suppress_col1")
        .short("1")
        .value_name("COL1")
        .takes_value(false)
        .help("Suppress printing of column 1"),
)
.arg(
    Arg::with_name("suppress_col2")
        .short("2")
        .value_name("COL2")
        .takes_value(false)
        .help("Suppress printing of column 2"),
)
.arg(
    Arg::with_name("suppress_col3")
        .short("3")
        .value_name("COL3")
        .takes_value(false)
        .help("Suppress printing of column 3"),
)
.arg(
    Arg::with_name("insensitive")
        .short("i")
        .value_name("INSENSITIVE")
        .takes_value(false)
        .help("Case insensitive comparison of
lines"),
)
.arg(
    Arg::with_name("delimiter")
        .short("d")
        .long("output-delimiter")
        .value_name("DELIM")
```



```
.help("Output delimiter")
    .takes_value(true),
)
    .get_matches();

    Ok(Config {
        file1:
        matches.value_of("file1").unwrap().to_string(),
        file2:
        matches.value_of("file2").unwrap().to_string(),
        suppress_col1: matches.is_present("suppress_col1"),
        suppress_col2: matches.is_present("suppress_col2"),
        suppress_col3: matches.is_present("suppress_col3"),
        insensitive: matches.is_present("insensitive"),
        delimiter:
        matches.value_of("delimiter").unwrap_or("\\t").to_string(),
    })
}
```

① 使用 Option::unwrap_or 来解包给定值或使用默认值（制表符），然后将值转换为 String。

此时，你的程序应该通过 cargo test dies_no_args。

## 验证和打开输入文件

首要任务是检查并打开输入文件。我建议对之前几章中使用的 open 函数做一个修改：

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ => Ok(Box::new(BufReader::new(
            File::open(filename)
                .map_err(|e| format!("{}: {}", filename, e))?,
        ))),
    }
}
```

① 将文件名纳入错误信息中。

这需要你相应地扩展导入：

```
use clap::{App, Arg};
use std::{
    error::Error,
    fs::File,
    io::{self, BufRead, BufReader},
};
```

如前所述，只允许其中一个输入为 “-” 表示 STDIN。你可以使用以下代码来检查文件名然后打开文件：

```
pub fn run(config: Config) -> MyResult<()> {
    let filename1 = &config.file1;
    let filename2 = &config.file2;

    if filename1.as_str() == "-" && filename2.as_str() == "-" { ①
        return Err(From::from("Both input files cannot be STDIN (\"-\")"));
    }

    let _file1 = open(&filename1)?; ②
    let _file2 = open(&filename2)?;
    println!("Opened {} and {}", filename1, filename2); ③

    Ok(())
}
```

① 检查两个文件名都不是 “-”。

② 尝试打开两个输入文件。

③ 打印一条消息以便你知道发生了什么。

你的程序应该拒绝两个 STDIN 参数：

```
$ cargo run -- - -
Both input files cannot be STDIN ("-")
```

对于两个良好的输入文件，它应该能够打印以下内容：

```
$ cargo run -- tests/inputs/file1.txt tests/inputs/file2.txt
Opened tests/inputs/file1.txt and tests/inputs/file2.txt
```

对于任一参数，它应该拒绝错误的文件：

```
$ cargo run -- tests/inputs/file1.txt blargh
blargh: No such file or directory (os error 2)
```

你应该通过 cargo test dies 的所有测试：

```
running 4 tests
test dies_both_stdin ... ok
test dies_no_args ... ok
test dies_bad_file1 ... ok
test dies_bad_file2 ... ok
```

## 处理文件

现在你的程序已经验证了所有参数并打开了输入文件，其中任何一个都可能是 STDIN。接下来，你需要遍历每个文件中的行以进行比较。你可以使用 BufRead::lines 来实现，因为不需要保留行结束符。从简单的情况开始，也许使用 empty.txt 文件（它是空的）和 file1.txt，它应该打印 file1.txt 中的所有行：

```
$ cd tests/inputs/
$ comm file1.txt empty.txt
```

```
a
b
c
d
```

确保如果你反转参数顺序，输出相同（但现在在第 2 列）：

```
$ comm empty.txt file1.txt
    a
    b
    c
    d
```

接下来，尝试使用 file1.txt 和 file2.txt。请注意，以下是 BSD comm 的输出，也是挑战程序的预期行为：

```
$ comm file1.txt file2.txt
        B
a
b
        c
d
```

GNU comm 在行不相等时显示行的顺序不同：

```
$ comm file1.txt file2.txt
a
b
        B
            c
d
```

这是在我自己编写版本之前一直觉得相当神秘的程序之一。我建议你先尝试从每个文件读取一行。BufRead::lines 的文档指出，当到达文件末尾时它会返回 None。

从其中一个参数为空文件开始将迫使你处理行数不相等的情况，此时你需要推进其中一个文件句柄而另一个保持不变。然后当你使用两个非空文件时，你必须考虑如何读取文件直到你有匹配的行并且独立移动它们，否则。

在你写完解决方案后，我会在另一边见到你。

## 解决方案

在我上次展示的版本基础上，我决定为每个文件的行创建迭代器，其中包含一个闭包来处理不区分大小写的比较：

```
pub fn run(config: Config) -> MyResult<()> {
    let filename1 = &config.file1;
    let filename2 = &config.file2;

    if filename1.as_str() == "-" && filename2.as_str() == "-" {
        return Err(From::from("Both input files cannot be STDIN (\"-\")"));
    }

    let case = |line: String| { ①
        if config.insensitive {
            line.to_lowercase()
        } else {
            line
        }
    };

    let mut lines1 = open(filename1)?.lines().filter_map(Result::ok).map(case); ②
    let mut lines2 = open(filename2)?.lines().filter_map(Result::ok).map(case);

    let line1 = lines1.next(); ③
    let line2 = lines2.next();
    println!("line1 = {:?}", line1); ④
    println!("line2 = {:?}", line2);

    Ok(())
}
```

- 1. 创建一个闭包，当 `config.insensitive` 为 true 时将每行文本转为小写。
- 2. 打开文件，创建一个 `lines` 迭代器以移除错误，然后将行映射通过 `case` 闭包。
- 3. 使用 `Iterator::next` 从每个文件句柄获取第一行。
- 4. 打印前两个值。

**注意**

在前面的代码中，我使用了函数 `Result::ok` 而不是编写闭包 `|line| line.ok()`。它们都能实现同样的功能，但前者更简短。

如我建议的那样，我将从其中一个文件为空开始：

```
$ cd ../..
$ cargo run -- tests/inputs/file1.txt tests/inputs/empty.txt
line1 = Some("a")
line2 = None
```

这让我思考如何根据两个迭代器可以得到的 `Some(line)` 和 `None` 的四种不同组合来遍历每个迭代器的行。在下面的代码中，我将可能性放在一个元组中，它是一个由括号包围的“有限异构序列”：

```
let mut line1 = lines1.next(); ①
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) { ②
        (Some(_val1), Some(_val2)) => { ③
            line1 = lines1.next();
            line2 = lines2.next();
        }
        (Some(_val1), None) => { ④
            line1 = lines1.next();
        }
        (None, Some(_val2)) => { ⑤
            line2 = lines2.next();
        }
        (None, None) => break, ⑥
    };
}
```

- ① 使行变量可变。
- ② 比较两个行变量的两种变体的所有可能组合。
- ③ 当两者都是 Some 值时，可以请求每个的下一行。
- ④ 当只有第一个值时，只请求第一个文件的下一行。
- ⑤ 对第二个文件做同样的操作。
- ⑥ 当两个文件中都没有行剩余时，跳出循环。

接下来，我必须在两者都有值时比较这两个值。当我只有来自第一个或第二个文件的值时，我应该分别在第一或第二列打印这些值。当它们相同时，我需要在第三列打印该值。当第一个值小于第二个值时，我应该在第一列打印第一个值；否则，我应该在第二列打印第二个值。为了理解最后一点，请考虑以下两个输入文件，我将它们并排放置，以便你想象代码如何读取行：

```
$ cat tests/inputs/file1.txt      $ cat tests/inputs/file2.txt
 a                                 B
 b                                 C
 c
 d
```

为了帮助你看到 BSD comm 的输出，我将输出通过管道传递给 Perl，用字符串 ---> 替换制表符，使其更清晰地显示打印了哪些列：

```
$ comm tests/inputs/file1.txt file2.txt | perl -pe "s/\t/--->/g"
--->B
a
b
--->--->c
d
```

现在想象你的代码从每个输入读取第一行，并得到来自 file1.txt 的 a 和来自 file2.txt 的 B。它们不相等，所以问题是打印哪一个。目标是模仿 BSD comm，所以我知道 B 应该先出现并打印在第二列。当我比较 a 和 B 时，我发现按代码点或数值排序时 a 大于 B。为了帮助你看到这一点，我在 util/ascii 中包含了一个程序，它将显示从第一个可打印字符开始的 ASCII 表的范围。请注意 a 的值为 97 而 B 为 66：

```
33: !   52: 4   71: G   90: Z   109: m
34: "   53: 5   72: H   91: [   110: n
```


| 35: # | 54: 6 | 73: I | 92: \ | 111: o |
|---|---|---|---|---|
| 36: $ | 55: 7 | 74: J | 93: ] | 112: p |
| 37: % | 56: 8 | 75: K | 94: ^ | 113: q |
| 38: & | 57: 9 | 76: L | 95: _ | 114: r |
| 39: ' | 58: : | 77: M | 96: ` | 115: s |
| 40: ( | 59: ; | 78: N | 97: a | 116: t |
| 41: ) | 60: < | 79: O | 98: b | 117: u |
| 42: * | 61: = | 80: P | 99: c | 118: v |
| 43: + | 62: > | 81: Q | 100: d | 119: w |
| 44: , | 63: ? | 82: R | 101: e | 120: x |
| 45: - | 64: @ | 83: S | 102: f | 121: y |
| 46: . | 65: A | 84: T | 103: g | 122: z |
| 47: / | 66: B | 85: U | 104: h | 123: { |
| 48: 0 | 67: C | 86: V | 105: i | 124: \| |
| 49: 1 | 68: D | 87: W | 106: j | 125: } |
| 50: 2 | 69: E | 88: X | 107: k | 126: ~ |
| 51: 3 | 70: F | 89: Y | 108: l | 127: DEL |

为了模仿 BSD comm，我应该先打印较小的值（B），然后从该文件再取一个值用于下一次迭代。GNU 版本则相反。注意你应该为此代码在导入中添加 use std::cmp::Ordering::*：

```
let mut line1 = lines1.next();
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) {
        (Some(val1), Some(val2)) => match
val1.cmp(val2) { ①
            Equal => { ②
                println!("{}", val1);
                line1 = lines1.next();
                line2 = lines2.next();
            }
            Less => { ③
                println!("{}", val1);
                line1 = lines1.next();
            }
            _ => { ④
                println!("{}", val2);
                line2 = lines2.next();
            }
        }
        (Some(val1), None) => {
            println!("{}", val1); ⑤
            line1 = lines1.next();
        }
        (None, Some(val2)) => {
            println!("{}", val2); ⑥
            line2 = lines2.next();
        }
        (None, None) => break,
    };
}
```

- 1. 使用 `std::cmp` 比较第一个值和第二个值。这将返回 `std::cmp::Ordering` 的一个变体。
- 2. 当两个值相等时，打印第一个值并从每个文件中获取值。
- 3. 当第一个文件的值小于第二个文件的值时，打印第一个值并请求第一个文件的下一个值。
- 4. 否则，打印第二个文件的值并请求第二个文件的下一个值。
- 5. 当只有第一个文件的值时，打印它并继续请求第一个文件的值。
- 6. 当只有第二个文件的值时，打印它并继续请求第二个文件的值。

如果我使用一个非空文件和一个空文件运行此代码，它可以正常工作：

```
$ cargo run -- tests/inputs/file1.txt
tests/inputs/empty.txt
a
b
c
d
```

如果我使用 *file1.txt* 和 *file2.txt*，结果与预期输出已经很接近了：

```
$ cargo run -- tests/inputs/file1.txt
tests/inputs/file2.txt
B
a
b
c
d
```

我决定创建一个名为 `Column` 的枚举来表示我应该在哪个列中打印值。每个变体都包含一个 `String`，你可以将以下内容放在 `src/lib.rs` 的顶部，靠近你的 `Config` 声明处。确保在导入中添加 `use crate::Column::*`，这样你就可以引用 `Col1` 而不是 `Column::Col1`：

```
enum Column {
    Col1(String),
    Col2(String),
    Col3(String),
}
```

接下来，我选择创建一个闭包来处理输出的打印。这也意味着我必须根据配置确定前两列的内容：

```
let default_col1 = if config.suppress_col1 {
    ""
} else {
    &config.delimiter
};

let default_col2 = if config.suppress_col2 {
    ""
} else {
    &config.delimiter
};
```

```
let printer = |col: Column| {
    let out = match col {
        Col1(val) => { ③
            if config.suppress_col1 {
                "".to_string()
            } else {
                val
            }
        }
        Col2(val) => format!( ④
            "{}{}",
            default_col1,
            if config.suppress_col2 { "" } else { &val }
        ),
        Col3(val) => format!( ⑤
            "{}{}{}",
            default_col1,
            default_col2,
            if config.suppress_col3 { "" } else { &val }
        ),
    };
    if !out.trim().is_empty() { ⑥
        println!("{}", out);
    }
};
```

- 1. 抑制列 1 时，使用空字符串；否则使用输出分隔符。
- 2. 对列 2 做同样的处理。
- 3. 给定列 1 的文本，决定是否打印或抑制输出。
- 4. 给定列 2 的文本，使用列 1 的默认值，并选择打印或抑制该列。
- 5 给定列 3 的文本，使用列 1 和列 2 的默认值，并选择打印或抑制该列。
- 6 仅打印非空的输出行。

我可以这样使用这个想法：

```
let mut line1 = lines1.next(); ①
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) {
        (Some(val1), Some(val2)) => match
val1.cmp(val2) {
            Equal => {
                printer(Col3(val1.to_string())); ②
                line1 = lines1.next();
                line2 = lines2.next();
            }
            Less => {
                printer(Col1(val1.to_string())); ③
                line1 = lines1.next();
            }
            _ => {
                printer(Col2(val2.to_string())); ④
                line2 = lines2.next();
            }
        },
        (Some(val1), None) => {
            printer(Col1(val1.to_string())); ⑤
            line1 = lines1.next();
        }
        (None, Some(val2)) => {
            printer(Col2(val2.to_string())); ⑥
            line2 = lines2.next();
        }
        (None, None) => break,
    };
}
```

① 从两个输入文件读取初始值。

- 2. 当值相同时，在列 3 中打印其中一个。
- 3. 当第一个值小于第二个值时，在列 1 中打印第一个值。
- 4. 否则，在列 2 中打印第二个值。
- 5. 当只有第一个文件的值时，在列 1 中打印它。
- 6. 当只有第二个文件的值时，在列 2 中打印它。

我喜欢有将输出分隔符从制表符更改为更明显的东西的选项：

```
$ cargo run -- -d="-->" tests/inputs/file1.txt
tests/inputs/file2.txt
--->B
a
b
--->--->c
d
```

经过这些更改，所有测试都通过了。

## 更进一步

- 修改程序以显示 GNU 输出并添加这些选项。
- 如我在章节介绍中所指出的，`comm` 对两个文件执行基本的连接操作，这与 `join` 程序类似。阅读该程序的手册页，并利用你编写 `commr` 的经验来编写 `joinr`。

## 总结

正如我所说，`comm` 对我来说一直是黑魔法，我每次都要查阅手册页来记住标志的含义。现在我写了自己的版本，我理解得好多了，而且非常喜欢它工作原理的优雅之处。考虑一下你学到了什么：

- 你可以通过在 `BufRead::lines` 上调用 `Iterator::next` 手动迭代文件句柄的行。
- 可以通过将可能性组合成元组来进行 `match`。
- 你可以使用 `std::cmp` 比较一个值和另一个值。结果是 `std::cmp::Ordering` 的一个变体。
- 你创建一个名为 `Column` 的 `enum`，其中变体可以保存字符串值，如 `Col1(String)`，这非常方便。
