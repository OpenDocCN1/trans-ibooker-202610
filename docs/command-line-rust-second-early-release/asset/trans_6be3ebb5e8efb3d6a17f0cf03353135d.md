
cat 工具在成功时退出码为 0，发生错误时退出码 >0。

## GNU 版本确实响应 --help：

```
$ cat --help
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s), or standard input, to standard output.

  -A, --show-all          equivalent to -vET
  -b, --number-nonblank   number nonempty output lines,
overrides -n
  -e                      equivalent to -vE
  -E, --show-ends         display $ at end of each line
  -n, --number            number all output lines
  -s, --squeeze-blank     suppress repeated empty output
lines
  -t                      equivalent to -vT
  -T, --show-tabs         display TAB characters as ^I
  -u                      (ignored)
  -v, --show-nonprinting  use ^ and M- notation, except
for LFD and TAB
      --help    display this help and exit
      --version output version information and exit
```

如果没有 FILE，或者 FILE 为 -，则读取标准输入。

示例：

cat f - g  Output f's contents, then standard input, then g's contents.
cat        Copy standard input to standard output.

GNU coreutils 在线帮助：
<http://www.gnu.org/software/coreutils/>
完整文档请运行：info coreutils 'cat invocation'

> NOTE
> 
> BSD 版本早于 GNU 版本，因此后者实现了所有相同的短标志以保持兼容性。如 GNU 程序的典型做法，它还提供了长标志别名，如 `-n` 的 `--number` 和 `-b` 的 `--number-nonblank`。我将向你展示如何像 GNU 版本一样提供这两种选项。

对于挑战程序，我将只实现选项 `-b|--number-nonblank` 和 `-n|--number`。我还将展示如何在文件名参数为 "-" 时读取常规文件和 `STDIN`。我已将四个测试文件放入 `03_catr/tests/inputs` 目录中：

- 1. *empty.txt*：一个空文件
- 2. *fox.txt*：一行文本
- 3. *spiders.txt*：三行文本
- 4. *the-bustle.txt*：艾米莉·狄金森的一首 lovely 诗，共九行包括一行空白

空文件很常见，虽然没什么用。我包含它是为了确保我的程序能够优雅地处理意外输入。也就是说，我希望我的程序至少不会崩溃。以下命令不产生输出，因此我期望我的程序也一样：

```
$ cd 03_catr
$ cat tests/inputs/empty.txt
```

接下来，我将在一个包含一行文本的文件上运行 `cat`：

```
$ cat tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

`-n|--number` 和 `-b|--number-nonblank` 标志都会对行进行编号，且行号在宽为六个字符的字段中右对齐，后跟一个制表符，然后是文本行。

为了区分制表符，我可以使用 -t 选项显示非打印字符，这样制表符会显示为 ^I。在以下命令中，我使用 Unix 管道 | 将第一个命令的 STDOUT 连接到第二个命令的 STDIN：

```
$ cat -n tests/inputs/fox.txt | cat -t
    1^IThe quick brown fox jumps over the lazy dog.
```

spiders.txt 文件有三行文本，应使用 -b 选项编号：

```
$ cat -b tests/inputs/spiders.txt
    1  Don't worry, spiders,
    2  I keep house
    3  casually.
```

-n（左侧）和 -b（右侧）之间的区别仅在 the-bustle.txt 上明显，因为后者只对非空白行编号：

```
$ cat -n tests/inputs/the-bustle.txt      $ cat -b tests/inputs/the-bustle.txt
    1  The bustle in a house                1  The bustle in a house
    2  The morning after death              2  The morning after death
    3  Is solemnest of industries           3  Is solemnest of industries
    4  Enacted upon earth,−                 4  Enacted upon earth,−
    5
    6  The sweeping up the heart,           5  The sweeping up the heart,
    7  And putting love away                6  And putting love away
    8  We shall not want to use again       7  We shall not want to use again
    9  Until eternity.                      8  Until eternity.
```

## NOTE

奇怪的是，你可以同时使用 -b 和 -n，且 -b 选项具有优先权。挑战程序将只允许其中一个。

当处理任何不存在或无法打开的文件时，cat 会向 STDERR 打印一条消息并继续处理下一个文件。在以下示例中，我使用 blargh 作为不存在的文件。我使用 touch 命令创建文件 cant-touch-this，并使用 chmod 命令将文件权限设置为使其无法读取。你将在第 15 章编写 ls 克隆时了解更多关于 000 的含义：

```
$ touch cant-touch-this && chmod 000 cant-touch-this
$ cat tests/inputs/fox.txt blargh tests/inputs/spiders.txt cant-touch-this
The quick brown fox jumps over the lazy dog. ①
cat: blargh: No such file or directory ②
Don't worry, spiders, ③
I keep house
casually.
cat: cant-touch-this: Permission denied ④
```

- ① 这是第一个文件的输出。
- ② 这是针对不存在文件的错误。
- ③ 这是第三个文件的输出。
- ④ 这是针对无法读取文件的错误。

最后，使用所有文件运行 cat，注意它会为每个文件重新编号行：

```
$ cd tests/inputs
$ cat -n empty.txt fox.txt spiders.txt the-bustle.txt
```

```
1  The quick brown fox jumps over the lazy dog.
1  Don't worry, spiders,
2  I keep house
3  casually.
1  The bustle in a house
2  The morning after death
3  Is solemnest of industries
4  Enacted upon earth,–
5
6  The sweeping up the heart,
7  And putting love away
8  We shall not want to use again
9  Until eternity.
```

如果你查看 *mk-outs.sh* 脚本，你会看到我使用 `cat` 以所有这些文件单独和一起的方式作为常规文件和通过 `STDIN`，使用无标志以及 `-n` 和 `-b` 标志。我将所有输出捕获到 *tests/expected* 目录中的各种文件中以用于测试。

## 开始使用测试驱动开发

在第 2 章中，我在章节末尾编写了测试，因为我需要向你展示语言的一些基础知识。从这个练习开始，我希望你思考 *test-driven development*（TDD），如 Kent Beck 所著的同名书籍（Addison-Wesley，2002）所述。TDD 提倡在编写代码 *之前* 编写代码的测试，如图 3-1 所示。

- 添加一个测试
- 运行所有测试并看到新测试失败
- 编写代码
- 运行测试
- 重构代码
- 重复

*图 3-1. 测试驱动开发循环*

## NOTE

从技术上讲，TDD 涉及在添加功能时编写每个测试。由于我已为你编写了所有测试，你可能认为这更像是 *test-first development*。一旦你编写了通过测试的代码，你就可以开始重构代码以改进它，也许通过缩短代码行或找到更快的实现。

你编写的挑战程序应命名为 `catr`（发音为 *cat-er*），即 `cat` 的 Rust 版本。我建议你使用 `cargo new catr` 开始一个新应用程序，然后将我的 *03_catr/tests* 目录复制到你的源树中。不要复制除测试之外的任何内容，因为你将自己编写其余代码。你应该具有如下结构：

```
$ tree -L 2 catr/
catr
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── cli.rs
    ├── expected
    └── inputs

4 directories, 3 files
```

我将使用第 2 章中的所有相同外部 crate，外加用于测试的 `rand` crate，因此将你的 *Cargo.toml* 更新为以下内容：

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

现在运行 `cargo test` 以下载 crate、编译你的程序并运行测试。所有测试都应该失败。你的任务，如果你选择接受，就是编写一个能够通过这些测试的程序。

## 创建库 crate

第 2 章中的程序相当简短，可以轻松放入 *src/main.rs*。你在职业生涯中编写的典型程序可能要长得多。从这个程序开始，我将把代码分为 *src/lib.rs* 中的库和调用库函数的 *src/main.rs* 中的二进制文件。我认为这种组织方式使得随着时间的推移更容易测试和扩展应用程序。

首先，我将把 *src/main.rs* 中的所有重要部分移动到 *src/lib.rs* 中的一个名为 `run` 的函数中。此函数将返回一种 `Result` 以指示成功或失败。这与第 2 章中的 `TestResult` 类型别名类似。`TestResult` 总是在 `Ok` 变体中返回单元类型 `()`，但 `MyResult` 可以返回包含任何类型的 `Ok`，我可以使用泛型 `T` 表示：

```
use std::error::Error; ①

type MyResult<T> = Result<T, Box<dyn Error>>;②

pub fn run() -> MyResult<()> { ③
    println!("Hello, world!"); ④
    Ok(()) ⑤
}
```

- ① 导入 `Error` trait 以表示错误值。
- ② 创建 `MyResult` 以表示任何类型 `T` 的 `Ok` 值或实现 `Error` trait 的某些 `Err` 值。
