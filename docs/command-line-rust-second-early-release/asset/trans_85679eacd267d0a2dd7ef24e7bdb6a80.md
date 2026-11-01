
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
