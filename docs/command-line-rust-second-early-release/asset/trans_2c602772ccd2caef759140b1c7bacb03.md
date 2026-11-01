
```
use tempfile::NamedTempFile; ①

type TestResult = Result<(), Box<dyn std::error::Error>>;

struct Test<'a> { ②
    input: &'a str,
    out: &'a str,
    out_count: &'a str,
}
```

① 用于创建临时输出文件。

② 用于定义输入文件以及有无计数时的预期输出值的结构体。

注意使用 'a 来表示值的生命周期。我想定义带有 &str 值的结构体，而 Rust 编译器希望确切地知道相对于彼此的值预计会保留多长时间。所有值都具有相同的生命周期 'a 这一事实意味着它们都预计会一样长久地存在。如果你删除生命周期注解并运行测试，你会看到与上一节中所示类似的编译器警告，以及关于如何修复它的确切建议：

```
error[E0106]: missing lifetime specifier
  --> tests/cli.rs:8:12
   |
8 |     input: &str,
   |            ^ expected named lifetime parameter
   |
help: consider introducing a named lifetime parameter
   |
7 | struct Test<'a> {
8 |     input: &'a str,
```

接下来，我定义测试所需的常量值：

```
const PRG: &str = "uniq r"; ①
```

```
const EMPTY: Test = Test {
    input: "tests/inputs/empty.txt", ②
    out: "tests/inputs/empty.txt.out", ③
    out_count: "tests/inputs/empty.txt.c.out", ④
};
```

- 1. 被测试程序的名称
- 2. 此测试的输入文件位置
- 3. 不带计数的输出文件位置
- 4. 带计数的输出文件位置

在 EMPTY 的声明之后，还有许多更多 Test 结构，后面跟着几个辅助函数。run 函数将使用 Test.input 作为输入文件，并将 STDOUT 与 Test.out 文件的内容进行比较：

```
fn run(test: &Test) -> TestResult { ①
    let expected = fs::read_to_string(test.out)?; ②
    Command::cargo_bin(PRG)? ③
        .arg(test.input)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

- 1. 该函数接受一个 Test 并返回一个 TestResult。
- 2. 尝试读取预期输出文件。
- 3. 尝试使用输入文件作为参数运行程序，验证其成功运行，并将 STDOUT 与预期值进行比较。

下一个辅助函数的工作方式非常相似，但这次测试计数功能：

```
fn run_count(test: &Test) -> TestResult {
    let expected = fs::read_to_string(test.out_count)?; ①
    Command::cargo_bin(PRG)?
        .args(&[test.input, "-c"]) ②
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

- ① 读取 `Test.out_count` 文件以获取预期输出。
- ② 将 `Test.input` 值和标志 `-c` 一起传递以统计行数。

下一个函数将通过 STDIN 传递输入：

```
fn run_stdin(test: &Test) -> TestResult {
    let input = fs::read_to_string(test.input)?; ①
    let expected = fs::read_to_string(test.out)?; ②
    Command::cargo_bin(PRG)? ③
        .write_stdin(input)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

- ① 尝试读取 `Test.input` 文件。
- ② 尝试读取 `Test.out` 文件。
- ③ 通过 STDIN 传递输入并验证 STDOUT 是否为预期值。

下一个函数同时测试从 STDIN 读取和统计行数：

```rust
fn run_stdin_count(test: &Test) -> TestResult {
    let input = fs::read_to_string(test.input)?;
    let expected = fs::read_to_string(test.out_count)?;
    Command::cargo_bin(PRG)? ①
        .arg("--count")
        .write_stdin(input)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

① 使用长选项 --count 运行程序并将输入馈送到 STDIN，验证 STDOUT 是否正确。

下一个函数检查程序是否接受输入和输出文件作为位置参数。这有点更有趣，因为我在测试中需要使用临时文件，因为正如你反复看到的那样，Rust 会并行运行测试。如果我使用同一个虚拟文件名比如 blargh 来写入所有输出文件，测试会覆盖彼此的输出。为了解决这个问题，我使用 tempfile::NamedTempFile 来获取一个动态生成的临时文件名，该文件在我完成时会自动删除：

```rust
fn run_outfile(test: &Test) -> TestResult {
    let expected = fs::read_to_string(test.out)?;
    let outfile = NamedTempFile::new()?; ①
    let outpath = &outfile.path().to_str().unwrap(); ②

    Command::cargo_bin(PRG)? ③
        .args(&[test.input, outpath])
        .assert()
        .success()
        .stdout("");
    let contents = fs::read_to_string(&outpath)?; ④
```

```
    assert_eq!(&expected, &contents); ⑤
    Ok(())
}
```

- 1. 尝试获取一个命名临时文件。
- 2. 获取文件的 `path`。
- 3. 使用输入和输出文件名作为参数运行程序，验证 `STDOUT` 中没有内容。
- 4. 尝试读取输出文件。
- 5. 检查输出文件的内容是否与预期值匹配。

接下来的两个函数是对我已经展示过的变体，添加了 `--count` 标志，最后要求程序在输入文件名是连字符时从 `STDIN` 读取。模块的其余部分使用各种结构体调用这些辅助函数来运行所有测试。写这一切很乏味，但非常重要的是用所有可能的命令行参数组合测试程序的每一个方面。你也许现在可以看到我为什么不想为挑战程序添加更多功能。

实现像 `uniq` 这样的开源程序的一个有趣方面是，你有时可以找到这些程序的源代码。在寻找原始测试套件时，我发现了一个用于测试 GNU 版本的 Perl 程序。我在 `tests/inputs/t[1-6].txt` 输入文件中使用了其中几个示例，如 *06_uniq r/mk-outs.sh* 中所示：

```
$ cat mk-outs.sh
#!/usr/bin/env bash

ROOT="tests/inputs"
```

```
OUT_DIR="tests/expected"

[[ ! -d "$OUT_DIR" ]] && mkdir -p "$OUT_DIR"

# Cf
https://github.com/coreutils/coreutils/blob/master/tests/misc/uniq.pl
echo -ne "a\na\n"      > $ROOT/t1.txt ①
echo -ne "a\na"        > $ROOT/t2.txt ②
echo -ne "a\nb"        > $ROOT/t3.txt ③
echo -ne "a\na\nb"     > $ROOT/t4.txt ④
echo -ne "b\na\na\n" > $ROOT/t5.txt ⑤
echo -ne "a\nb\nc\n" > $ROOT/t6.txt ⑥

for FILE in $ROOT/*.txt; do
  BASENAME=$(basename "$FILE")
  uniq          $FILE > ${OUT_DIR}/${BASENAME}.out
  uniq -c       $FILE > ${OUT_DIR}/${BASENAME}.c.out
  uniq          < $FILE > ${OUT_DIR}/${BASENAME}.stdin.out
  uniq -c < $FILE > ${OUT_DIR}/${BASENAME}.stdin.c.out
done
```

- ① 每行各以换行符结尾的两行
- ② 最后一行没有尾随换行符
- ③ 两个不同的行，没有尾随换行符
- ④ 两个相同的行，最后一个不同且没有尾随换行符
- ⑤ 每个都有换行符的两个不同值
- ⑥ 每个都有换行符的三个不同值

我发现精确匹配 `uniq` 的输出非常具有挑战性，但我想我现在因为这项努力而变得更好了。我建议从 `src/lib.rs` 开始读取输入文件，再次使用前几章中的 `open` 函数是有意义的：

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

请确保扩展你的导入以包含以下内容：

```
use clap::{App, Arg};
use std::{
    error::Error,
    fs::File,
    io::{self, BufRead, BufReader},
};
```

① 这种语法让我按通用前缀分组导入，因此以下所有内容都来自 std。

你可以借用第三章中相当多的代码，该代码从输入文件或 STDIN 读取文本行，同时保留行尾：

```
pub fn run(config: Config) -> MyResult<()> {
    let mut file = open(&config.in_file)
        .map_err(|e| format!("{}: {}", config.in_file,
e))?; ①
    let mut line = String::new(); ②
    loop { ③
        let bytes = file.read_line(&mut line)?; ④
        if bytes == 0 { ⑤
            break;
        }
        print!("{}", line); ⑥
        line.clear(); ⑦
    }
    Ok(())
}
```

①
