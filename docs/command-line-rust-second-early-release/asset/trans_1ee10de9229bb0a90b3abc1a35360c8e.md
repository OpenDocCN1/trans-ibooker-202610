
(i.e. force
    grep to behave as egrep).

-e pattern, --regexp=pattern
    指定在搜索输入时使用的模式：输入
    行如果匹配任何指定的
模式则会被选中。
    当使用多个 -e 选项来
    指定多个模式，或当模式以连字符
    （`-`）开头时，此选项最有用。

这些选项的相反是：

-G, --basic-regexp
    将模式解释为基本正则表达式
（即强制 grep
    表现得像传统 grep）。

正则表达式自 1950 年代以来就已存在，当时由美国数学家 Stephen Cole Kleene² 发明。自那时起，语法已被各个团体修改和扩展，其中最值得注意的是 Perl 社区创建了 Perl 兼容正则表达式（PCRE）。默认情况下，grep 只会解析基本正则表达式，但前面的标志可以让它使用其他变体。例如，我可以使用模式 ee 来搜索包含两个相邻 e 的任何行：

```
$ grep 'ee' tests/inputs/*
tests/inputs/bustle.txt:The sweeping up the heart,
```

如果我想查找任何重复两次的字符，模式是 (.)\1，其中点号 (.) 代表任意字符，而捕获括号允许我使用反向引用 \1 来引用
第一个捕获组。这是扩展表达式的一个例子，因此需要 -E 标志：

```
$ grep -E '(.)\1' tests/inputs/*
tests/inputs/bustle.txt:The sweeping up the heart,
tests/inputs/bustle.txt:And putting love away
tests/inputs/bustle.txt:We shall not want to use again
tests/inputs/nobody.txt:Are you–Nobody–too?
tests/inputs/nobody.txt:Don't tell! they'd advertise–you know!
tests/inputs/nobody.txt:To tell one's name–the livelong June–
```

Rust `regex` crate 文档指出其“语法类似于 Perl 风格的正则表达式，但缺少一些功能，如环视和反向引用。”（*Look-around assertions* 允许表达式断言模式必须跟随或先于另一个模式，而 *backreferences* 允许模式引用先前捕获的值。）这意味着挑战程序在处理扩展正则表达式时默认会更像 `egrep`。遗憾的是，这也意味着程序将无法处理前面的模式，因为它需要反向引用。它仍然是一个很酷的程序可以编写，所以让我们开始吧。

## 查找要搜索的文件

为了使用编译好的正则表达式，我接下来需要找到所有要搜索的文件。请记住，用户可能会使用 -- `recursive` 选项提供目录名称以搜索每个目录中包含的所有文件；否则，目录名称应导致警告打印到 `STDERR`。我决定编写一个名为 `find_files` 的函数，它将接受一个字符串向量，这些字符串可能是文件或目录名称，以及一个布尔值，表示是否递归进入目录。它返回一个 `MyResult` 值的向量，其中要么保存有效文件的名称字符串，要么保存错误消息：

```
fn find_files(files: &[String], recursive: bool) ->
Vec<MyResult<String>> {
    unimplemented!();
}
```

为了测试这一点，我可以在 `src/lib.rs` 中添加一个 `tests` 模块。请注意，这将使用 `rand` 模块，该模块应列在 *Cargo.toml* 的 `[dev-dependencies]` 部分中，如本章前面所述：

```
#[cfg(test)]
mod tests {
    use super::find_files;
    use rand::{distributions::Alphanumeric, Rng};

    #[test]
    fn test_find_files() {
        // Verify that the function finds a file known to exist
        let files =
            find_files(&
["./tests/inputs/fox.txt".to_string()], false);
        assert_eq!(files.len(), 1);
        assert_eq!(files[0].as_ref().unwrap(),
"./tests/inputs/fox.txt");

        // The function should reject a directory without
the recursive option
        let files = find_files(&
["./tests/inputs".to_string()], false);
        assert_eq!(files.len(), 1);
        if let Err(e) = &files[0] {
            assert_eq!(
                e.to_string(),
                "./tests/inputs is a directory".to_string()
            );
        }

        // Verify the function recurses to find four files
in the directory
        let res = find_files(&
["./tests/inputs".to_string()], true);
        let mut files: Vec<String> = res
```

```
.iter()
.map(|r| r.as_ref().unwrap().replace("\\", "/"))
.collect();
files.sort();
assert_eq!(files.len(), 4);
assert_eq!(
    files,
    vec![
        "./tests/inputs/bustle.txt",
        "./tests/inputs/empty.txt",
        "./tests/inputs/fox.txt",
        "./tests/inputs/nobody.txt",
    ]
);

// Generate a random string to represent a
nonexistent file
    let bad: String = rand::thread_rng()
        .sample_iter(&Alphanumeric)
        .take(7)
        .map(char::from)
        .collect();

// Verify that the function returns the bad file as
an error
    let files = find_files(&[bad], false);
    assert_eq!(files.len(), 1);
    assert!(files[0].is_err());
}
}
```

你应该能够运行 cargo test test_find_files 来验证你的函数是否能找到现有文件、正确递归，并将不存在的文件报告为错误。以下是我在代码中如何使用它：

```
pub fn run(config: Config) -> MyResult<()> {
    println!("pattern \"{}\"", config.pattern);

    for entry in find_files(&config.files,
config.recursive) {
        match entry {
```

```
Err(e) => eprintln!("{}", e),
Ok(filename) => println!("file \"{}\"",
filename),
}
}

Ok(())
}
```

我的解决方案使用 WalkDir，我在第 7 章 (findr) 中介绍过它。
看看你是否能让你的程序重现以下输出。
首先，默认输入应为 "-" 以表示从 STDIN 读取：

```
$ cargo run -- fox
pattern "fox"
file "-"
```

## 注意

打印正则表达式意味着调用 Regex::as_str 方法。
Regex::build 指出这“将逐字生成传递给 new 的模式。
值得注意的是，它不会合并在此构建器上设置的任何标志。”

显式列出 "-" 作为输入应产生相同的输出：

```
$ cargo run -- fox -
pattern "fox"
file "-"
```

程序应处理多个输入文件：

```
$ cargo run -- fox tests/inputs/*
pattern "fox"
file "tests/inputs/bustle.txt"
file "tests/inputs/empty.txt"
file "tests/inputs/fox.txt"
file "tests/inputs/nobody.txt"
```

没有 --recursive 选项的目录名称应被拒绝：

```
$ cargo run -- fox tests/inputs
pattern "fox"
tests/inputs is a directory
```

使用 --recursive 标志时，它应找到目录的文件：

```
$ cargo run -- -r fox tests/inputs
pattern "fox"
file "tests/inputs/empty.txt"
file "tests/inputs/nobody.txt"
file "tests/inputs/bustle.txt"
file "tests/inputs/fox.txt"
```

不存在的参数应在处理每个条目时打印到 STDERR：

```
$ cargo run -- -r fox blargh tests/inputs/fox.txt
pattern "fox"
blargh: No such file or directory (os error 2)
file "tests/inputs/fox.txt"
```

## 查找匹配的输入行

一旦你正确处理输入，就该打开文件并搜索匹配的行了。我建议你再次使用前面章节中的 open 函数，该函数将打开并读取现有文件或文件名 "-" 的 STDIN。你需要为此添加 use std::fs::File 和 use std::io::{self, BufRead, BufReader}：

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

读取行时，请确保保留行结束符，因为其中一个输入文件包含 Windows 风格的 CRLF 行结束符。我的解决方案使用一个名为 `find_lines` 的函数，你可以从以下内容开始：

```
fn find_lines<T: BufRead>(
    mut file: T, ①
    pattern: &Regex, ②
    invert_match: bool, ③
) -> MyResult<Vec<String>> {
    unimplemented!();
}
```

- ① `file` 必须实现 `std::io::BufRead` trait。
- ② `pattern` 是对编译后正则表达式的引用。
- ③ `invert_match` 是一个布尔值，用于是否反转匹配操作。

注意

在第 5 章中，我使用 `impl BufRead` 来表示必须实现 `BufRead` 的值。在前面的代码中，我使用 `<T: BufRead>` 来为类型 T 编写 trait 边界。

为了测试此函数，我通过添加以下 `test_find_lines` 函数扩展了我的 `tests` 模块，该函数再次使用 `std::io::Cursor` 创建一个实现 `BufRead` 的假文件句柄用于测试：

```
#[cfg(test)]
mod test {
    use super::{find_files, find_lines};
    use rand::{distributions::Alphanumeric, Rng};
    use regex::{Regex, RegexBuilder};
```
