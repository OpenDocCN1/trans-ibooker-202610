
```
.conflicts_with_all(&["chars", "bytes"]),
)
.arg(
    Arg::with_name("bytes") ④
        .value_name("BYTES")
        .help("Selected bytes")
        .short("b")
        .long("bytes")
        .conflicts_with_all(&["fields", "chars"]),
)
.arg(
    Arg::with_name("chars") ⑤
        .value_name("CHARS")
        .help("Selected characters")
        .short("c")
        .long("chars")
        .conflicts_with_all(&["fields", "bytes"]),
)
.get_matches();
```

- ① 必需的文件接受多个值且默认为破折号。
- ② 分隔符使用制表符作为默认值。
- ③ fields 选项与 chars 和 bytes 冲突。
- ④ bytes 选项与 fields 和 chars 冲突。
- ⑤ chars 选项与 fields 和 bytes 冲突。

接下来，我将分隔符转换为字节并验证只有一个：

```
let delimiter =
    matches.value_of("delimiter").unwrap_or("\t");
    let delim_bytes = delimiter.as_bytes();
    if delim_bytes.len() > 1 {
        return Err(From::from(format!(
            "--delim \"{}\" must be a single byte",
            delimiter
        )));
}
```

我使用 parse_pos 函数来处理所有可选的列表值：

```
let fields =
    matches.value_of("fields").map(parse_pos).transpose()?;
    let bytes =
    matches.value_of("bytes").map(parse_pos).transpose()?;
    let chars =
    matches.value_of("chars").map(parse_pos).transpose()?;
```

> NOTE
> 
> 我在这里引入 Option::transpose，它“将 Result 的 Option 转置为 Option 的 Result。”

然后我确定要创建哪个 Extract 变体。我不应该触发此代码中的 else 分支，但保留它也很好：

```
let extract = if let Some(field_pos) = fields {
        Fields(field_pos)
    } else if let Some(byte_pos) = bytes {
        Bytes(byte_pos)
    } else if let Some(char_pos) = chars {
        Chars(char_pos)
    } else {
        return Err(From::from("Must have --fields, --bytes, or --chars"));
    };
```

如果代码执行到此点，那么我似乎拥有可以返回的有效参数：

```
Ok(Config {
    files: matches.values_of_lossy("files").unwrap(),
    delimiter: delim_bytes[0],
    extract,
```
```
})
}
```

接下来，你需要弄清楚如何使用这些信息从输入中提取所需的部分。

## 提取字符或字节

在第 4 章 (headr) 和第 5 章 (wcr) 中，你学习了如何在文件中处理行、字节和字符。你应该借鉴这些程序来帮助你在本次挑战中选择字符和字节。一个区别是无需保留行尾，因此你可以使用 BufRead::lines 来读取输入文本的行。

开始时，你可以考虑引入之前使用过的 open 函数来帮助打开每个文件：

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

你可以扩展 run 以处理正常和异常的文件：

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

这将需要一些额外的导入：

```rust
use crate::Extract::*;
use clap::{App, Arg};
use regex::Regex;
use std::{
    error::Error,
    fs::File,
    io::{self, BufRead, BufReader},
};
```

现在考虑如何从文件句柄的每一行中提取字符。我编写了一个名为 `extract_chars` 的函数，它将返回由给定索引位置的字符组成的新字符串：

```rust
fn extract_chars(line: &str, char_pos: &[usize]) -> String
{
    unimplemented!();
}
```

> **TIP**
> 
> 我使用 `&[usize]` 而不是 `&PositionList` 是因为 cargo clippy 的建议：

```
warning: writing `&Vec<_>` instead of `&[_]` involves one more reference
and cannot be used with non-Vec-based slices
  --> src/lib.rs:214:40
   |
214 | fn extract_chars(line: &str, char_pos: &PositionList)
   -> String {
   |
   |                     ^^^^^^^^^^^^^
   |
   = note: `# [warn(clippy::ptr_arg)]` on by default
   = help: for further information visit
https://rust-lang.github.io/rust-clippy/master/index.html#ptr_arg
```

下面是一个你可以添加到 tests 模块的测试，以帮助你了解该函数的工作方式：

```
#[test]
fn test_extract_chars() {
    assert_eq!(extract_chars("", &[0]), "".to_string());
    assert_eq!(extract_chars("áb c", &[0]),
"á".to_string());
    assert_eq!(extract_chars("áb c", &[0, 2]),
"ác".to_string());
    assert_eq!(extract_chars("áb c", &[0, 1, 2]),
"áb c".to_string());
    assert_eq!(extract_chars("áb c", &[2, 1]),
"cb".to_string());
    assert_eq!(extract_chars("áb c", &[0, 1, 4]),
"áb".to_string());
}
```

我还编写了一个类似的 extract_bytes 函数来解析字节：

```
fn extract_bytes(line: &str, byte_pos: &[usize]) -> String
{
    unimplemented!();
}
```

下面是测试：

```
#[test]
fn test_extract_bytes() {
    assert_eq!(extract_bytes("áb c", &[0]),
"�".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1]),
"á".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 2]),
"áb".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 2, 3]),
"áb c".to_string());
    assert_eq!(extract_bytes("áb c", &[3, 2]),
"cb".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 5]),
"á".to_string());
}
```

一旦你编写了这两个函数使其通过给定的测试，就迭代输入文本的行并使用前面的函数提取并打印每一行所需的字符或字节。当你运行 `cargo test` 时，你应该能够通过除以下之外的所有测试：

```
failures:
    csv_f1
    csv_f1_2
    csv_f1_3
    csv_f2
    csv_f2_3
    csv_f3
    tsv_f1
    tsv_f1_2
    tsv_f1_3
    tsv_f2
    tsv_f2_3
    tsv_f3
```

## 解析分隔文本文件

要通过最终测试，你需要学习如何解析分隔文本文件。如前所述，此文件格式使用逗号或制表符之类的分隔符来指示字段的边界。有时分隔字符也可能是数据的一部分，在这种情况下，字段应用引号括起来以转义分隔符。正确解析分隔文本的最简单方法是使用类似 `csv` 模块的东西。我强烈建议你先阅读教程。

接下来，考虑以下示例，展示如何使用此模块解析分隔数据。如果你想编译并运行此代码，请新建一个项目并使用以下内容作为 *src/main.rs*。请确保在 *Cargo.toml* 中添加 `csv` 依赖项并将 *books.csv* 文件复制到项目中。

```
use csv::StringRecord;
use std::fs::File;
```

```
fn main() -> std::io::Result<()> {
    let mut reader = csv::ReaderBuilder::new() ①
        .delimiter(b',') ②
        .from_reader(File::open("books.csv")?); ③

    fmt(reader.headers()?); ④
    for record in reader.records() { ⑤
        fmt(&record?);
    }

    Ok(())
}

fn fmt(rec: &StringRecord) {
    println!(
        "{}",
        rec.into_iter() ⑥
            .map(|v| format!("{:20}", v)) ⑦
            .collect::<Vec<String>>() ⑧
            .join("")
    )
}
```

- ① 使用 `csv::ReaderBuilder` 解析文件。
- ② `delimiter` 必须是一个单个 `u8` 字节。
- ③ `from_reader` 方法接受一个实现 `Read` trait 的值。
- ④ `Reader::headers` 将以 `StringRecord` 的形式返回第一行中的列名。
- ⑤ `Reader::records` 方法提供对 `StringRecord` 值迭代器的访问。
- ⑥ `StringRecord` 中的字段值可以被迭代。
- ⑦ 使用 `Iterator::map` 将值格式化为宽 20 个字符的字段。
- ⑧ 将字符串收集到向量中并连接成新的字符串以便打印。

如果我运行此程序，我将看到以下输出：

```
$ cargo run
Author      Year    Title
Émile Zola  1865    La Confession de
Claude
Samuel Beckett  1952    Waiting for Godot
Jules Verne 1870    20,000 Leagues
Under the Sea
```

回到挑战程序，思考如何使用这些想法编写类似 `extract_fields` 的函数，该函数接受一个 `StringRecord` 并提取 PositionList 中找到的字段：

```
fn extract_fields(record: &StringRecord, field_pos: & [usize]) -> Vec<String> {
    unimplemented!();
}
```

下面是一个你可以使用 的测试：

```
#[test]
fn test_extract_fields() {
    let rec = StringRecord::from(vec!["Captain", "Sham", "12345"]);
    assert_eq!(extract_fields(&rec, &[0]), &["Captain"]);
    assert_eq!(extract_fields(&rec, &[1]), &["Sham"]);
    assert_eq!(extract_fields(&rec, &[0, 2]), &["Captain", "12345"]);
    assert_eq!(extract_fields(&rec, &[0, 3]), &["Captain"]);
    assert_eq!(extract_fields(&rec, &[1, 0]), &["Sham", "Captain"]);
}
```
