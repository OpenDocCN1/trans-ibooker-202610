
另一个选项是 [创建 Makefile](https://github.com/noahgift/rust-new-project-template/blob/main/Makefile#L1-L7) 并在其中放入关键命令，例如 `make rust-version`，它会检查 `cargo` 和 `rust` 的版本。你可以看到有多个工具可以帮助你在 Rust 中完成工作：

```
rust-version:
	@echo "Rust command-line utility versions:"
	rustc --version 			#rust compiler
	cargo --version 			#rust package manager
	rustfmt --version			#rust code formatter
	rustup --version			#rust toolchain manager
	clippy-driver --version		#rust linter
```

为了以后在本地运行所有内容，你可以执行 `make all`，它将格式化/检查/测试所有项目 [在此仓库](https://github.com/noahgift/rust-new-project-template/blob/main/Makefile#L24)。

接下来，要构建一个 hello-world 示例，你可以使用内置的 `cargo` 命令。Cargo 是 Rust 生态系统中最重要和最有价值的部分之一。

创建项目目录：`cargo new hello`。`cargo` 命令会创建一个结构，你可以用 `tree hello` 查看：

```
hello/
├── Cargo.toml
└── src
    └── main.rs
1 directory, 2 files
```

`Cargo.toml` 文件是项目配置所在的位置，即如果你需要添加依赖。源代码文件在 `main.rs` 中的内容如下。它看起来像 Python 或其他任何现代语言，并且此函数会打印一条消息。

```
fn main() {
    println!("Hello, world MLOPs!");
}
```

运行项目时，你 `cd` 进入 `hello` 并运行 `cargo run`。输出如下所示：

```
@noahgift > /workspaces/rust-mlops-template/hello (main) $ cargo run
   Compiling hello v0.1.0 (/workspaces/rust-mlops-template/hello)
    Finished dev [unoptimized + debuginfo] target(s) in 0.36s
     Running `target/debug/hello`
Hello, world MLOPs!
```

最后，以无噪音的方式运行：`cargo run --quiet`。如果你想运行生成的二进制文件，以下命令会执行它 `./target/debug/hello`。从 Python 角度来看，这有多么出色非常重要，因为你的代码的二进制分发是免费的。在 Python 中，二进制可执行文件的概念并不是原生 Python 支持的工作流程。相反，要实现相同的可移植可执行文件，最接近的方式需要将 Python 打包到 Docker 工作流程中。

通过这个 hello world 项目可以学到的一大要点是，Rust 生态系统处理了许多复杂的编程部分，即在 Rust [Cargo 包管理器](https://doc.rust-lang.org/cargo/) 内的 linting、测试、格式化和二进制部署。如果 Rust 代码通过 lint 并编译成功，运行时应该就能工作。对于 Python 来说则不能这样说，因为它没有编译器。除了更可靠之外，它还能提供“C-level”速度，同时具有与 Python 相似的可读语法。最后，语言的许多特性，如不可变变量和合理的并发设计，使代码更安全，因为编译器不会编译不安全代码。

还有一个工具可以使 Rust 编程更有效，即在代码创建的初始阶段包含 [GitHub Copilot](https://github.com/features/copilot)，或类似的工具如 [Amazon CodeWhisperer](https://aws.amazon.com/codewhisperer/)。Copilot 与强大的 Cargo 生态系统的协同作用是提高生产力的秘诀，如 [图 10-3](#ch10.xhtml#Figure-8-5-modern-rust-development) 所示。

![](assets/8-5-modern-rust-development.png)

此外，通过在工作流程中包含 [GitHub Actions](https://github.com/features/actions) 的自动化，这些必要的自动化步骤也会作为 CI/CD 的一部分发生。MLOps 的基础是在构建系统上发生的软件工程最佳实践的自动化。这种核心能力使后续的进一步自动化成为可能：

```
name: Rust CI/CD Pipeline
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
env:
  CARGO_TERM_COLOR: always
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: actions-rs/toolchain@v1
      with:
          toolchain: stable
          profile: minimal
          components: clippy, rustfmt
          override: true
    - name: update linux
      run: sudo apt update
    - name: update Rust
      run: make install
    - name: Check Rust versions
      run: make rust-version
    - name: Format
      run: make format
    - name: Lint
      run: make lint
    - name: Test
      run: make test
```

`Makefile` 是项目中优秀组件的原因之一是，本地的步骤——安装、linting、格式化和测试——在构建系统上以相同的方式运行。这种方法消除了自动构建软件时可能出现的错误。

这种 Rust 开发风格既新颖，又是开发者生产力的重要进步。借助 GitHub Copilot 和 AWS Codewhisper 等工具，切换到 Rust、C# 或 Go 等更高性能语言会更易于管理。这种新的软件工程形式使 *prompt engineering* 成为软件开发中一个有效且合理的第一阶段。接下来，由 Rust Cargo 工具格式化的代码允许像 Copilot 这样的服务进行“重新审视”，从而可能提高建议的质量。

一旦该阶段完成，Cargo lint 工具 Clippy 会进一步深入代码质量并对其进行增强。最后，运行测试确保业务逻辑正常工作，然后最后一步编译生成最优且安全的代码。

接下来，让我们超越简单的 hello-world 工具，进入 PyTorch 和 Hugging Face。

# 使用 PyTorch 和 Hugging Face 与 Rust

[Hugging Face](https://huggingface.co/) 是一个用于构建大型语言模型（LLM）解决方案的新兴平台，它可与 Rust 和 PyTorch 配合使用。对于此示例，我使用预训练的摘要模型拼凑了一个相当逼真的演示工具类型示例；示例 [项目位于此处](https://github.com/noahgift/rust-mlops-template/tree/main/sqlite-hf)。此演示的总体结构如下。歌曲 Maná 的《En El Muelle De San Blas》的歌词位于 `lyrics.txt` 文件中。

```
├── Cargo.toml
├── Makefile
├── README.md
├── lyrics.txt
└── src
  ├── lib.rs
  └── main.rs

2 directories, 6 files
```

你可以在 [图 10-4](#ch10.xhtml#Figure-8.9-sqlite-hugging-face-pytorch) 中看到总体架构。思路是 Rust 与 SQLite 数据库的性能相结合，而 Hugging Face 则是使用预训练 LLM 分析歌词的可重复模式。

![](assets/8.9-sqlite-hugging-face-pytorch.png)

自然语言处理（NLP）中的一个常见问题是构建分析文本的解决方案。以下示例展示了如何制作一个可重复、高效且安全的解决方案来分析歌词。
