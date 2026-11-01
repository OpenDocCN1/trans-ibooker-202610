
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
