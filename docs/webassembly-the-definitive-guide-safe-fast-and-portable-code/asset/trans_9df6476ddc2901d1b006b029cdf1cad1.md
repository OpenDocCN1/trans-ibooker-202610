

```
brian@tweezer ~/g/w/s/ch11> wasicc -o hello hello.c
brian@tweezer ~/g/w/s/ch11> wasm3 hello.wasm
Hello, World!
brian@tweezer ~/g/w/s/ch11> wasmer hello.wasm
Hello, World!
brian@tweezer ~/g/w/s/ch11> wasmtime hello.wasm
Hello, World!
brian@tweezer ~/g/w/s/ch11> ./hello
Hello, World!
```

我们现在已经使用相同的运行时运行了在 Rust 和 C 中未经修改编译的代码。相同的 WASI 形式的 WebAssembly 模块也可在 Linux 或 Windows 上运行。只要我们的 WASI 启用主机满足我们的期望，我们应该可以在 WebAssembly 运行的任何地方运行。注意 Wasienv 甚至为我们生成了一个独立的原生应用程序！你将在下一章了解这意味着什么。

事实证明，Node.js 和 Deno 也都支持 WASI。甚至还有一个 JavaScript 兼容层，可以让你的 WASI 启用应用程序在浏览器中运行！请参阅图 11-1，了解 WASI 兼容层在实际中的示例。这可在 https://wasi.dev/polyfill/ 获取。

![](img/16b7990eb0dbf589db177bdbd2f4a189_236_0.png)

图 11-1. 在浏览器中的兼容层中运行 WASI 应用程序

WebAssembly 使你的代码具有可移植性。WASI 通过在你需要时满足其期望来使你的应用程序具有可移植性。

## 基于能力的<|message|>

WASI 的另一个方面对于 WebAssembly 在这种无处不在的计算环境中的未来至关重要。我们希望能够向我们想要提供功能的应用提供功能，但不提供给恶意软件。由于我们通常无法区分两者，我们很可能会将受限的沙箱环境作为默认。

从一开始，WASI 就被设想为一种手段，用于对托管模块的行为施加基于能力的访问限制。

简而言之，这意味着，就像 WebAssembly 不给任意代码直接访问内存一样，WASI 不给它直接访问敏感资源（如文件句柄、网络套接字或子进程详情）的权限。相反，资源将通过不可伪造的、不透明的句柄暴露出来，向代码提供能力。由于这需要将语言特定的或可能是操作系统级别的结构在 WebAssembly 模块之间传递，我们需要一种方法来创建外部引用。

如何以及为何这会起作用，在本书撰写时仍是一个复杂且不断变化的目标。我一直在为自己纠结要关注多少细节，最终决定现在其中大部分内容会成为一种干扰，部分原因是这些提案仍在进行中。这也是因为它在某些方面超级复杂，即使它逐渐发展成一个越来越优雅的设计。在第 12 章我会让你了解更多细节，但现在，让我们专注于看到结果。

在示例 11-4 中，我们有一个用 Rust 编写的简单程序，它将一个字符串写入文件然后读回。从编程角度来看，它非常简单。真正的大问题，当然是访问文件系统。如果你还不是 Rust 大师，不要为细节而烦恼，程序的流程应该仍然有意义。

### 示例 11-4. 使用文件系统的 Rust 程序

```
use std::fs;
use std::io::{Read, Write};

fn main() {
    let greeting = "Hello, world!";
    let outfile = "hello.txt";
    let mut output_file = fs::File::create(outfile)
        .expect(&format!("error creating {}", outfile));

    output_file.write_all(greeting.as_bytes())
        .expect(&format!("Error writing: {}", outfile));

    let mut input_file = fs::File::open(outfile)
        .expect(&format!("error opening {}", outfile));

    let mut input = String::new();
    input_file.read_to_string(&mut input)
        .expect(&format!("Error reading: {} ", outfile));

    println!("Read in from file: {}", input);
}
```

我将此代码放在另一个 cargo 驱动的项目中，因此生成原生可执行文件轻而易举。

```
brian@tweezer ~/g/w/s/c/hello-fs> cargo build --release
    Finished release [optimized] target(s) in 0.01s
brian@tweezer ~/g/w/s/c/hello-fs> cargo run --release
    Finished release [optimized] target(s) in 0.01s
    Running `target/release/hello-fs`
Read in from file: Hello, world!
brian@tweezer ~/g/w/s/c/hello-fs> ls -alF
total 32
drwxr-xr-x  10 brian  staff   320 Jun 27 13:18 ./
drwxr-xr-x  10 brian  staff   320 Jun 27 18:42 ../
-rw-r--r--   1 brian  staff   177 Jun 27 12:18 Cargo.toml
-rw-r--r--   1 brian  staff    13 Jun 27 18:47 hello.txt
drwxr-xr-x   3 brian  staff    96 Jun 27 13:18 src/
drwxr-xr-x@  7 brian  staff   224 Jun 27 18:42 target/
brian@tweezer ~/g/w/s/c/hello-fs> cat hello.txt
Hello, world!
```

如你所见，我们可以构建并运行程序。文件被创建并读回一个字符串，我们将其打印到控制台。这是因为，作为这台机器的主要用户和管理员，我有权限执行这些操作。

如果我从互联网下载一个随机程序并运行它会怎样？那可能不是个好主意。我被赋予的权限通过我们称之为混淆代理问题¹¹ 的场景授予了随机代码。如果它是一个恶意应用程序，它可以删除文件、窃取我的比特币私钥¹² 或加密我的硬盘以勒索赎金。

然而，让我们重新编译应用程序以针对 WASI 并尝试在 Wasmtime 中运行它。

```
brian@tweezer ~/g/w/s/c/hello-fs> cargo build --target wasm32-wasi --release
Finished release [optimized] target(s) in 0.01s
brian@tweezer ~/g/w/s/c/hello-fs> wasmtime target/wasm32-wasi/release/hello-fs.wasm
thread 'main' panicked at 'error creating hello.txt: Custom {
kind: Other, error: "failed to find a
pre-opened file descriptor through which \"hello.txt\" could be
opened" }', src/main.rs:8:10
note: run with `RUST_BACKTRACE=1` environment variable to display
a backtrace
Error: failed to run main module `target/wasm32-wasi/release/hello-fs.wasm`

Caused by:
    0: failed to invoke command default
    1: wasm trap: unreachable
      wasm backtrace:
        0: 0x838a - <unknown>!__rust_start_panic
        1: 0x7fd5 - <unknown>!rust_panic
        2: 0x7c42 - <unknown>!std::panicking::rust_panic_with_hook::h914d916f8f3c1673
        3: 0x7190 - <unknown>!std::panicking::begin_panic_handler::{closure}::h15c8896377a9b562
        4: 0x70d1 - <unknown>!std::sys_common::backtrace::__rust_end_short_backtrace::h8e2b0a38bc469b94
        5: 0x7790 - <unknown>!rust_begin_unwind
        6: 0xdc62 - <unknown>!core::panicking::panic_fmt::h79dd662186e4ad97
        7: 0xebb2 - <unknown>!core::result::unwrap_failed::hc8762c9cd74198d4
        8: 0xce3 - <unknown>!hello_fs::main::h13132a06338f22dc
        9: 0x73d - <unknown>!std::sys_common::backtrace::__rust_begin_short_backtrace::h3c3458f2edddefd1
        10: 0xe02 - <unknown>!std::rt::lang_start::{closure}::h4a57af2dc5415a53
        11: 0x8139 - <unknown>!std::rt::lang_start_internal::hb132ad43e5d53599
        12: 0xdc9 - <unknown>!__original_main
        13: 0x44d - <unknown>!_start
        14: 0x11bff - <unknown>!_start.command_export
      note: run with `WASMTIME_BACKTRACE_DETAILS=1` environment variable to display more information
```

嗯，这没起作用。有很多噪音，但关键点是我们没有执行我们想要执行的操作的能力，因为 WASI 主机尚未以不可伪造的句柄的形式赋予我们这种能力（在上面的错误消息中称为“预打开文件描述符”）。

重新启动可执行文件但调用允许目录访问的命令行参数解决了问题。

```
brian@tweezer ~/g/w/s/c/hello-fs> wasmtime --dir=. target/wasm32-wasi/release/hello-fs.wasm
Read in from file: Hello, world!
```

Wasmer 以稍微不同的方式表述消息，但问题是相同的。

```
brian@tweezer ~/g/w/s/c/hello-fs> wasmer target/wasm32-wasi/release/hello-fs.wasm
thread 'main' panicked at 'error creating hello.txt: Os
  { code: 2, kind: PermissionDenied, message: "Permission denied" }', src/main.rs:8:10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: failed to run `target/wasm32-wasi/release/hello-fs.wasm`
  1: WASI execution failed
  2: failed to run WASI `_start` function
  3: RuntimeError: unreachable
      at __rust_start_panic (hello-fs.wasm[171]:0x838a)
      at rust_panic (hello-fs.wasm[163]:0x7fd5)
      at
std::panicking::rust_panic_with_hook::h914d916f8f3c1673 (hello-fs.wasm[156]:0x7c42)
      at std::panicking::begin_panic_handler::
{{closure}}::h15c8896377a9b562 (hello-fs.wasm[145]:0x7190)
      at
std::sys_common::backtrace::__rust_end_short_backtrace::h8e2b0a38bc469b94 (hello-fs.wasm[144]:0x70d1)
      at rust_begin_unwind (hello-fs.wasm[155]:0x7790)
      at core::panicking::panic_fmt::h79dd662186e4ad97 (hello-fs.wasm[239]:0xdc62)
      at core::result::unwrap_failed::hc8762c9cd74198d4 (hello-fs.wasm[262]:0xebb2)
      at hello_fs::main::h13132a06338f22dc (hello-fs.wasm[17]:0xce3)
      at
std::sys_common::backtrace::__rust_begin_short_backtrace::h3c3458f2edddefd1
```

