
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
