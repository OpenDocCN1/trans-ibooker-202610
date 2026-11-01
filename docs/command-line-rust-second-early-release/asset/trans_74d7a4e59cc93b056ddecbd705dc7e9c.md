
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

