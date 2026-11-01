
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
