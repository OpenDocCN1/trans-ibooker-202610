# 第 1 章 真相与后果

### 早期发布读者的说明

在早期发布电子书中，您可以获得书籍的最早形式——作者在写作时的原始未编辑内容，以便您在这些标题正式发布之前就能利用这些技术。

这将是最终书籍的第 1 章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何评论，或在本章中发现缺失的材料，请联系作者 kyclark@gmail.com。

> *真相是我们什么都不知道*
> —They Might Be Giants

在本章中，我将向您展示如何组织、运行和测试 Rust 程序。我将使用 Unix 平台（macOS）来解释有关系统程序的一些基本概念。其中一些概念仅适用于 Windows 操作系统，但 Rust 程序本身无论使用哪个平台都将以相同的方式工作。

您将学到：
- 如何将 Rust 代码编译为可执行文件
- 如何使用 Cargo 开始新项目
- 关于 `$PATH` 环境变量
- 如何从 crates.io 使用外部 Rust crate
- 关于程序的退出状态
- 如何使用常见的系统命令和选项
- 如何编写 `true` 和 `false` 程序的 Rust 版本
- 如何组织、编写和运行测试

### 从“Hello, world!”开始

开始的地方就是开头，似乎大家普遍认为这意味着向屏幕打印“Hello, world！” 。您可以使用 `cd /tmp` 切换到临时目录来编写第一个程序，然后启动文本编辑器并将此 Rust 程序输入到名为 *hello.rs* 的文件中：

```
fn main() { ①
    println!("Hello, world!"); ②
} ③
```

① 函数使用 `fn` 定义。此函数的名称为 `main`。

② `println!` （*打印行*）用于向 `STDOUT` （读作 *标准输出*）打印。分号表示语句的结束。

③ 函数体用大括号括起来。

Rust 会自动在 `main` 函数中启动。函数参数出现在其后的括号内。因为 `main()` 中没有列出参数，所以该函数不接受参数。我在这里要指出的最后一件事是 `println!` 看起来像一个函数，但实际上是一个 `macro`，本质上是编写代码的代码。本书中您还会看到其他以感叹号结尾的宏，例如 `assert!`。

要运行此程序，您必须先使用 Rust 编译器 `rustc` （或 Windows 上的 `rustc.exe`）将代码 *编译* 为计算机可执行的形式：

```
$ rustc hello.rs
```

如果一切顺利，前一个命令将没有输出，但您现在应该有一个名为 *hello.exe* 的新文件（Windows）或 *hello* （macOS 和 Linux）。在 macOS 上，我可以使用 `file` 命令查看这是什么类型的文件：

```
$ file hello
hello: Mach-O 64-bit executable x86_64
```

您应该可以直接执行它以查看一个迷人而真挚的信息：

```
$ ./hello ①
Hello, world!
```

① 点号（.）表示当前目录。

提示

由于安全问题，有必要使用显式路径指示当前目录中的可执行文件。

在 Windows 上，您可以这样执行：

```
> .\hello.exe
Hello, world!
```

那很酷。

### 组织 Rust 项目目录

在您的 Rust 项目中，您可能会编写许多源代码文件，并且还会使用来自 crates.io 等来源的代码。最好创建一个目录来包含所有这些内容。我将创建一个名为 hello 的目录，并在其中创建一个 src 目录用于 Rust 源代码文件：

```
$ rm hello ①
$ mkdir -p hello/src ②
```

① 删除 hello 二进制文件，以便我可以创建 hello 目录。

② mkdir 命令将创建目录。-p 选项表示在创建子目录之前创建父目录。

现在我将 hello.rs 源文件移入 hello/src：

```
$ mv hello.rs hello/src
```

进入该目录并再次编译程序：

```
$ cd hello
$ rustc src/hello.rs
```

您应该再次在目录中拥有 hello 可执行文件。我将使用 tree 命令（您可能需要安装）来向您展示我的目录内容：

```
$ tree
.
├── hello
│   └── src
│       └── hello.rs
```

这是一个简单 Rust 项目的基本结构。

## 使用 Cargo 创建和运行项目

实际上，如果你使用我在前言中介绍的 Cargo 工具来启动新的 Rust 项目会更容易。移出你的 *hello* 目录并删除它：

```
$ cd .. ①
$ rm -rf hello ②
```

① `cd` 命令会 *更改目录*。两个点 (..) 表示父目录。

② `rm` 命令会删除文件或空目录。`-r` *递归* 选项会删除目录的内容，而 `-f` *强制* 选项会跳过任何错误。

开始使用 Cargo 重新创建项目，如下所示：

```
$ cargo new hello
    Created binary (application) `hello` package
```

这应该会创建一个新的 *hello* 目录，你可以进入其中。我将再次使用 `tree` 来显示其内容：

```
$ cd hello
$ tree
.
├── Cargo.toml
└── src
    └── main.rs
```

## 注意

使用 Cargo 启动项目不是必须的，但强烈建议使用，几乎所有 Rust 社区的人都在使用。如果你愿意，也可以自己创建目录结构和文件。

你可以看到 Cargo 创建了一个包含文件 *main.rs* 的 *src* 目录。我将使用 `cat` 命令（*连接*，你将在第 3 章编写）来显示此文件的内容，应该看起来很熟悉：

```
$ cat src/main.rs
fn main() {
    println!("Hello, world!");
}
```

与其使用 `rustc` 编译程序，我希望你使用 **cargo run** 在一个命令中编译源代码并运行它：

```
$ cargo run
    Compiling hello v0.1.0 (/private/tmp/hello) ①
    Finished dev [unoptimized + debuginfo] target(s) in
1.26s
    Running `target/debug/hello`
Hello, world! ②
```

- ① 前三行是关于 Cargo 正在做什么的信息。
- ② 这是程序的输出。

如果你希望 Cargo 更安静一些，可以使用 `-q` 或 `--quiet` 选项：

```
$ cargo run --quiet
Hello, world!
```

## CARGO 命令

我是怎么知道这个选项的？如果不带其他参数运行 cargo，它会打印文档，内容如下所示：

```
$ cargo
Rust's package manager

USAGE:
    cargo [+toolchain] [OPTIONS] [SUBCOMMAND]

OPTIONS:
    -V, --version                Print version info and exit
    --list                       List installed commands
    --explain <CODE>             Run `rustc --explain CODE`
    -v, --verbose                Use verbose output (-vv very verbose/build.rs output)
    -q, --quiet                  No output printed to stdout
    --color <WHEN>               Coloring: auto, always, never
    --frozen                     Require Cargo.lock and cache are up to date
    --locked                     Require Cargo.lock is up to date
    --offline                    Run without accessing the network
    --config <KEY=VALUE>...      Override a configuration value (unstable)
    -Z <FLAG>...                 Unstable (nightly-only) flags to Cargo, see cargo -Z help for details
    -h, --help                   Prints help information
```

这体现了优秀系统工具的一个关键特性：它们通常会告诉你如何使用它们，就像《爱丽丝梦游仙境》中的饼干上写着“吃我”。请注意，前面的输出中第一个词之一是 *USAGE*，因此通常称之为 *用法*。本书中的程序也会打印其用法。继续阅读，你会看到可以请求 Cargo 任何命令的 *帮助*：

```
Some common cargo commands are (see all commands with --list):
    build, b      Compile the current package
    check, c      Analyze the current package and report errors, but don't
                  build object files
    clean         Remove the target directory
    doc           Build this package's and its dependencies' documentation
    new           Create a new cargo package
    init          Create a new cargo package in an existing directory
    run, r        Run a binary or example of the local package
    test, t       Run the tests
    bench         Run the benchmarks
    update        Update dependencies listed in Cargo.lock
    search        Search registry for crates
    publish       Package and upload this package to the registry
    install       Install a Rust binary. Default location is $HOME/.cargo/bin
    uninstall     Uninstall a Rust binary

See 'cargo help <command>' for more information on a specific command.
```

使用 Cargo 运行程序后，我可以使用 `ls` *列表* 命令（你将在第 15 章编写）来查看现在是否有一个名为 *target* 的新目录：

```
$ ls
Cargo.lock  Cargo.toml  src/        target/
```

你可以使用之前的 tree 命令或 find 命令（你将在第 7 章编写）来查看 Cargo 和 Rust 创建的所有文件。运行的可执行文件应存在于 target/debug/hello。你可以直接运行它，就像之前的程序一样：

```
$ ./target/debug/hello
Hello, world!
```

因此，在没有任何指导的情况下，Cargo 成功找到了 src/main.rs 中的源代码，并使用其中的 main 函数构建了一个名为 hello 的程序然后运行它。为什么二进制文件仍然叫做 hello，而不是 main 呢？要回答这个问题，请查看 Cargo.toml¹，这是项目的配置文件：

```
$ cat Cargo.toml
[package]
name = "hello" ①
version = "0.1.0" ②
edition = "2018" ③

[dependencies]
```

- ① 这是我用 Cargo 创建的项目的名称，所以它也将是可执行文件的名称。
- ② 这是程序的版本。
- ③ 这是 Rust 的版本。

这里有几件事需要讨论。首先，Rust crate 预计使用语义化版本号，如 major.minor.patch，因此 1.2.4 是主版本 1、次版本 2、补丁版本 4。其次，Rust 版本是一个社区引入不向后兼容更改的方式。我将在本书的所有程序中使用 2018 版本。

## 编写和运行集成测试

> *测试不仅仅是测试行为，设计测试的行为是已知最好的错误预防方法之一。创建有用测试所需的思考可以在编码前发现并消除错误——事实上，测试设计思维可以在软件创建的每个阶段发现并消除错误，从构思到规范、设计、编码等等。*
> 
> —Boris Beizer, 软件测试技术

我希望向你展示的一个重要部分是测试代码的价值。尽管这个程序很简单，仍然有一些需要验证的内容。我使用的测试有几个大类，我可以描述为 *由内而外*，其中我为程序内部的函数编写测试，以及 *由外而内*，其中我编写测试以像用户那样运行程序。第一种测试通常称为 *单元* 测试，因为函数是编程的基本单元。我将在第 2 章向你展示如何编写这些测试。对于这个程序，我想从后一种测试开始，这通常称为 *集成* 测试，因为它检查程序作为一个整体是否正常工作。

为集成测试代码创建 *tests* 目录是很常见的。这可以让你的源代码保持整洁有序，并且在不进行测试时也便于编译器忽略这段代码：

```
$ mkdir tests
```

我想通过在命令行上运行它来测试 `hello` 程序，就像用户会做的那样，所以我将为 *命令行界面*（CLI）创建文件 *tests/cli.rs*。你的项目现在应该如下所示：

```
$ tree -L 2
.
├── Cargo.lock
├── Cargo.toml
├── src
│   └── main.rs
├── target
│   ├── CACHEDIR.TAG
│   └── debug
└── tests
    └── cli.rs
```

首先添加这个函数，它展示了 Rust 中测试的基本结构：

```
#[test] ①
fn works() {
    assert!(true); ②
}
```

- ① #[test] 属性告诉 Rust 在测试时运行此函数。
- ② assert! 宏断言布尔表达式为真。

本书中的所有测试都将使用 assert! 来验证某个期望为真，或使用 assert_eq! 来验证某物是否为预期值。由于此测试评估的字面值为 true，它将始终成功。要查看此测试的运行情况，请执行 cargo test。你应该在输出中看到以下几行：

```
    Running tests/cli.rs (target/debug/deps/cli-
27c6c9a94ed7c7df)

running 1 test
test works ... ok
```

要观察失败的测试，请将 true 改为 false：

```
#[test]
fn works() {
    assert!(false);
}
```

在输出中，你应该会看到以下失败的测试：

```
running 1 test
test works ... FAILED
```

> 提示

> 你可以在一个测试函数中使用任意多个 `assert!` 和 `assert_eq!` 调用。如果其中任何一个失败，则整个测试都会失败。

仅仅断言 `true` 和 `false` 没有用处，所以删除该函数。相反，我将检查 `hello` 程序是否可以执行。我将使用 `std::process::Command` 来执行命令并检查结果。首先，我将演示使用命令 `ls`，我知道它在 Unix 和 Windows 上都有效：

```
use std::process::Command; ①

#[test]
fn runs() {
    let mut cmd = Command::new("ls"); ②
    let res = cmd.output(); ③
    assert!(res.is_ok()); ④
}
```

- ① 导入用于创建新命令的 `std::process::Command` 结构体。
- ② 创建一个新命令来运行 `ls`。使用 `mut` 使此变量 *可变*，因为它会改变。
- ③ 运行命令并捕获输出，它将是一个 `Result`。
- ④ 验证结果是否为 `Ok` 值。

> 注意

> 默认情况下，Rust 变量是不可变的，这意味着它们的值不能被改变。

运行 `cargo test` 并验证在所有输出中你能看到一个通过的测试：

```
running 1 test
test runs ... ok
```

尝试将函数更改为执行 `hello` 而不是 `ls`：

```
#[test]
fn runs() {
    let mut cmd = Command::new("hello");
    let res = cmd.output();
    assert!(res.is_ok());
}
```

再次运行测试并注意它会失败，因为找不到 `hello` 程序：

```
running 1 test
test runs ... FAILED
```

回想一下，二进制文件存在于 *target/debug/hello* 中。如果你在命令行尝试执行 `hello`，你会看到程序找不到：

```
$ hello
-bash: hello: command not found
```

当你执行任何命令时，你的操作系统会在一组预定义的目录中查找该名称²的东西。在类 Unix 系统上，你可以检查 shell 的 PATH 环境变量来查看此目录列表，这些目录由冒号分隔。（在 Windows 上，这是 $env:Path。）我可以使用 tr（转换字符）将冒号 (:) 替换为换行符 (\n) 来显示我的 PATH：

```
$ echo $PATH | tr : '\n' ①
/opt/homebrew/bin
/Users/kyclark/.cargo/bin
/Users/kyclark/.local/bin
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

① $PATH 告诉 bash 插值该变量。使用管道 | 将其传递给 tr。

即使我切换到 target/debug 目录，由于上述排除当前工作目录的安全限制，hello 仍然找不到：

```
$ cd target/debug/
$ ls hello
hello*
$ hello
-bash: hello: command not found
```

我仍然必须显式引用路径：

```
$ ./hello
Hello, world!
```

## 添加项目依赖

目前，`hello` 程序只存在于 *target/debug* 目录中。如果我将其复制到我的 `PATH` 中的任何目录（注意我为私有程序包含了 `$HOME/.local/bin` 目录），我就可以执行它并成功运行测试。我不想复制我的程序来测试它；相反，我想测试位于当前 crate 中的程序。我可以使用 crate `assert_cmd` 在我的 crate 目录中找到该程序。我首先需要将其作为开发依赖项添加到 *Cargo.toml*。这告诉 Cargo 我只需要此 crate 用于测试和基准测试：

```
[package]
name = "hello"
version = "0.1.0"
edition = "2018"

[dependencies]

[dev-dependencies]
assert_cmd = "1"
```

然后我可以使用此 crate 创建一个 `Command`，它会在 Cargo 二进制目录中查找。下面的测试不会验证程序是否产生正确的输出，只会验证它似乎成功。更新你的 `runs` 函数，使用以下定义：

```
use assert_cmd::Command; ①

#[test]
fn runs() {
    let mut cmd = Command::cargo_bin("hello").unwrap(); ②
    cmd.assert().success(); ③
}
```

- 1. 导入 `assert_cmd::Command`。
- 2. 创建一个 `Command` 在当前 crate 中运行 `hello`。这会返回一个 `Result`，我假设使用 `Result::unwrap` 是安全的，因为二进制文件已找到。
- 3. 执行程序并使用 `Assert::success` 来“确保命令成功。”

> **注意**
> 我将在后面的章节中更多地谈论 `Result` 类型。现在，只需知道这是一种建模可能成功或失败的东西的方式，其中有两个可能的变体，分别是 `Ok` 和 `Err`。

## 理解程序退出值

程序成功退出意味着什么？系统程序应向操作系统报告最终的退出状态。可移植操作系统接口（POSIX）标准规定，标准退出代码为 0 表示成功（想想零错误），否则为 1 到 255 之间的任何数字。我可以使用 `bash` shell 和 `true` 命令来向你展示这一点。以下是 macOS 上存在的版本的 `man true` 手册页：

```
TRUE(1)                                 BSD General Commands Manual
TRUE(1)

NAME
    true -- Return true value.

SYNOPSIS
    true

DESCRIPTION
    The true utility always returns with exit code zero.
```

## 另请参阅

csh(1), sh(1), false(1)

## 标准

The true utility conforms to IEEE Std 1003.2-1992 ('`POSIX.2''').

BSD
June 27, 1991
BSD

正如文档所示，此程序什么都不做但成功。如果我运行 true，我看不到任何输出，但我可以检查 bash 变量 $? 来查看最近程序的退出状态：

```
$ true
$ echo $?
0
```

`false` 命令是其对应物，因为它“总是以非零退出代码退出”：

```
$ false
$ echo $?
1
```

本书中你将编写的所有程序都应期望在正常终止时返回值为 0，在出现错误时返回非零值。我可以编写自己的 true 和 false 版本来向你展示如何做到这一点。首先创建 src/bin 目录，然后创建 src/bin/true.rs 并使用以下内容：

```
$ cat src/bin/true.rs
fn main() {
    std::process::exit(0); ①
}
```

①

使用 `std::process::exit` 函数使程序以值 0 退出。

你的 src 目录现在应具有以下结构：

```
$ tree src/
src/
├── bin
│   └── true.rs
└── main.rs
```

你可以运行程序并手动检查退出值：

```
$ cargo run --quiet --bin true ①
$ echo $?
0
```

① `--bin` 选项是“要运行的 bin 目标的名称。”

将以下测试添加到 *tests/cli.rs* 以确保其正常工作：

```
#[test]
fn true_ok() {
    let mut cmd = Command::cargo_bin("true").unwrap();
    cmd.assert().success();
}
```

如果运行 `cargo test`，你应该会看到以下输出：

```
running 2 tests
test true_ok ... ok
test runs ... ok
```

## 注意

测试不一定按在代码中声明的顺序运行。这是因为 Rust 是编写 *并发* 代码的安全语言，这意味着代码可以在多个线程上运行。测试利用这种并发性并行运行许多测试，因此每次运行测试时测试结果可能以不同的顺序出现。这是一个特性，而不是错误。如果你想按顺序运行测试，可以通过 `cargo test -- --test-threads=1` 在单个线程上运行它们。

Rust 程序默认会以代码 0 退出。回想一下，*src/main.rs* 并未显式调用 `std::process::exit`。这意味着 `true` 程序可以什么都不做。要确定吗？将 *src/bin/true.rs* 更改为以下内容：

```
$ cat src/bin/true.rs
fn main() {}
```

运行测试套件并验证它仍然通过。接下来，让我们编写 `false` 程序的版本，使用以下源代码：

```
$ cat src/bin/false.rs
fn main() {
    std::process::exit(1); ①
}
```

① 使用 1-255 之间的任何值退出以表示错误。

验证这对你来说是否看起来正确：

```
$ cargo run --quiet --bin false
$ echo $?
1
```

将此测试添加到 *tests/cli.rs* 以验证程序失败：

```
#[test]
fn false_not_ok() {
    let mut cmd = Command::cargo_bin("false").unwrap();
    cmd.assert().failure(); ①
}
```

① 使用 Assert::failure 函数来“确保命令失败。”

运行 cargo test 以验证所有程序都按预期工作：

```
running 3 tests
test runs ... ok
test true_ok ... ok
test false_not_ok ... ok
```

另一种编写此程序的方式使用 std::process::abort：

```
$ cat src/bin/false.rs
fn main() {
    std::process::abort();
}
```

再次运行测试套件以确保程序仍然按预期工作。

## 测试程序输出

很高兴知道我的 hello 程序能正确退出，我还想确保它确实将正确的输出打印到 STDOUT（读作 standard out），这是输出通常出现的地方，一般是控制台。将 tests/cli.rs 中的 runs 函数更新为以下内容：

```
#[test]
fn runs() {
    let mut cmd = Command::cargo_bin("hello").unwrap();
    cmd.assert().success().stdout("Hello, world!\n");
}
```

① 验证命令成功退出并将给定文本打印到 STDOUT。

运行测试并验证 hello 确实正常工作。接下来，修改 src/main.rs 以添加更多感叹号：

```
$ cat src/main.rs
fn main() {
    println!("Hello, world!!!");
}
```

再次运行测试以观察失败的测试：

```
running 3 tests
test true_ok ... ok
test false_not_ok ... ok
test runs ... FAILED

failures:

---- runs stdout ----
thread runs panicked at 'Unexpected stdout, failed diff var original
|__ original: Hello, world!
|__ diff:
--- value     expected
+ value actual
@@ -1 +1 @@
-Hello, world!
+Hello, world!!!
|__ var as str: Hello, world!!!
```

学习阅读测试输出本身就是一项技能，但输出正努力向你展示预期输出和 *实际* 输出是什么。虽然这是一个微不足道的程序，但我希望你能看到自动检查我们编写的程序各个方面的价值。

## 退出值使程序可组合

正确报告退出状态是良好行为的系统程序的特征。退出值很重要，因为与另一个进程一起使用的失败进程应导致组合失败。例如，我可以在 bash 中使用逻辑 *和* 运算符 && 来链接 true 和 ls 两个命令。只有当第一个进程报告成功时，第二个进程才会运行：

```
$ true && ls
Cargo.lock  Cargo.toml  src/        target/     tests/
```

如果我执行 false && ls，则第一个进程失败且 ls 从未执行。此外，整个命令的退出状态为非零。

```
$ false && ls
$ echo $?
1
```

正确报告错误的系统程序使它们可与其他程序组合。这很重要，因为在 Unix 环境中，将许多小型命令组合成命令行上的 *临时* 程序是极其常见的。如果程序遇到错误但未能向操作系统报告，则结果可能不正确。最好让程序中止，以便可以修复潜在问题。

## 总结

本章旨在向你介绍有关组织 Rust 项目的一些关键思想以及有关系统程序的一些基本思想。以下是你应该理解的一些内容：

- Rust 编译器 `rustc` 可以将 Rust 源代码编译成 Windows、macOS 和 Linux 上的机器可执行文件。
- Cargo 工具可以帮助创建 Rust 项目。你也可以用它来编译、运行和测试代码。
- 你看到了 `ls`、`cd`、`mkdir` 和 `rm` 等系统工具的几个示例，它们接受诸如文件或目录名之类的参数以及诸如 `-f` 或 `-p` 之类的选项。
- POSIX 兼容的程序应以值 `0` 退出以表示成功，以 1-255 之间的任意值表示错误。
- 你编写了 `hello` 程序来说“Hello, world！”并看到了如何测试其退出状态和打印到 `STDOUT` 的文本。
- 你学习了如何向 *Cargo.toml* 添加 crate 依赖并在代码中使用这些 crate。
- 你创建了 *tests* 目录来组织测试代码，并使用 `#[test]` 使函数成为测试。
- 你学习了通过在 *src/bin* 目录中创建源代码文件，在 Cargo 项目中编写、运行和测试替代二进制文件。
- 你编写了 `true` 和 `false` 程序的自己的实现以及测试以验证它们按预期成功和失败。你看到默认情况下 Rust 程序将以值 `0` 退出，并且可以使用 `std::process::exit` 函数显式地以给定代码退出。此外，可以使用 `std::process::abort` 函数以错误代码退出。

1. TOML 是 Tom’s Obvious, Minimal Language 的缩写。
2. Shell 别名和函数也可以像命令一样执行，但我现在只谈论查找要运行的程序。
