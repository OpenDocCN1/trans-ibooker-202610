

转换为可以进一步转换为高效原生表示的通用形式。

我们的 LLVM 安装应原生支持 WebAssembly 作为后端。为双重检查，尝试以下操作：

```
brian@tweezer ~/g/w/s/ch05> llc --version
LLVM (http://llvm.org/):
    LLVM version 11.0.1
Optimized build.
Default target: x86_64-apple-darwin20.2.0
Host CPU: skylake

Registered Targets:
    aarch64      - AArch64 (little endian)
    aarch64_32   - AArch64 (little endian ILP32)
    aarch64_be   - AArch64 (big endian)
    arm          - ARM
    arm64        - ARM64 (little endian)
    arm64_32     - ARM64 (little endian ILP32)
    nvptx        - NVIDIA PTX 32-bit
    nvptx64      - NVIDIA PTX 64-bit
    ppc32        - PowerPC 32
    ppc64        - PowerPC 64
    ppc64le      - PowerPC 64 LE
    r600         - AMD GPUs HD2XXX-HD6XXX
    riscv32      - 32-bit RISC-V
    riscv64      - 64-bit RISC-V
    wasm32       - WebAssembly 32-bit
    wasm64       - WebAssembly 64-bit
    x86          - 32-bit X86: Pentium-Pro and above
    x86-64       - 64-bit X86: EM64T and AMD64
    xcore        - XCore
```

我已为长度编辑了支持的目标列表（它更长！），但仍想展示大多数主要平台都受支持。为了简化我们的即时使用，我将把程序中独立的 main() 功能替换为仅下面的 age 计算函数。

## 示例 5-3. 仅 howOld 函数。

```
int howOld(int currentYear, int yearBorn) {
    int retValue = -1;
```
```
if(yearBorn <= currentYear) {
    retValue = currentYear - yearBorn;
}

return retValue;
}
```

要将其编译为 WebAssembly，我们可以使用以下命令：

```
brian@tweezer ~/g/w/s/ch05> clang --target=wasm32 -nostdlib -Wl,--no-entry ↵
-Wl,--export-all howold2.c -o howold.wasm
```

`--target=wasm32` 指令针对 32 位 WebAssembly 平台。`-nostdlib` 告诉它不要链接标准库，因为我们最初并不打算在函数可直接使用的位置运行该函数（即浏览器）。`--no-entry` 和 `--export-all` 指令告诉链接器不要期望 main() 函数并保留所有函数以供导出。没有后者的提示，优化过程可能会消除未使用的函数，因为从技术上讲没有任何东西在调用它们。`-o howold.wasm` 指定输出文件。

我们剩下的是一个可工作的 Wasm 模块，我们知道如何从前几章中探索和使用它。文件中有很多新的噪音，但基本情况保持不变。我们拥有类型、函数、内存以及我们目前将忽略的各种内存管理细节。

```
brian@tweezer ~/g/w/s/ch05> wasm-objdump -x howold.wasm

howold.wasm:    file format wasm 0x1

Section Details:

Type[2]:
  - type[0] () -> nil
  - type[1] (i32, i32) -> i32
Function[2]:
  - func[0] sig=0 <__wasm_call_ctors>
  - func[1] sig=1 <howOld>
Table[1]:
```
```
- table[0] type=funcref initial=1 max=1
Memory[1]:
  - memory[0] pages: initial=2
Global[7]:
  - global[0] i32 mutable=1 - init i32=66560
  - global[1] i32 mutable=0 <__dso_handle> - init i32=1024
  - global[2] i32 mutable=0 <__data_end> - init i32=1024
  - global[3] i32 mutable=0 <__global_base> - init i32=1024
  - global[4] i32 mutable=0 <__heap_base> - init i32=66560
  - global[5] i32 mutable=0 <__memory_base> - init i32=0
  - global[6] i32 mutable=0 <__table_base> - init i32=1
Export[9]:
  - memory[0] -> "memory"
  - func[0] <__wasm_call_ctors> -> "__wasm_call_ctors"
  - func[1] <howOld> -> "howOld"
  - global[1] -> "__dso_handle"
  - global[2] -> "__data_end"
  - global[3] -> "__global_base"
  - global[4] -> "__heap_base"
  - global[5] -> "__memory_base"
  - global[6] -> "__table_base"
Code[2]:
  - func[0] size=2 <__wasm_call_ctors>
  - func[1] size=134 <howOld>
Custom:
  - name: "name"
  - func[0] <__wasm_call_ctors>
  - func[1] <howOld>
Custom:
  - name: "producers"
```

在示例 5-4 中，我们使用新的模块根据 HTML 输入范围设置计算年龄。这显然不是我们必须用 C 编写的函数，但我们暂时保持简单。我们有一个 range `<input>` 元素，其最大值在 WebAssembly 模块加载后设置为当前年份。我们任意将最小值设置为过去 100 年。我们有一个名为 updateLabels 的函数，用于在值更改时设置元素上的值，以及另一个函数用于在滑块值更改时重新计算某人的年龄。监听函数调用我们的模块，使用 currentYear 和滑块的当前值来计算差值。

### 示例 5-4. 在 HTML 中使用 howOld 函数。

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <link rel="stylesheet" href="bootstrap.min.css">
        <title>How Old Are You?</title>
        <script src="utils.js"></script>
    </head>
    <body>
        <div id="container" class="container" style="width: 80%">
            <h1>How Old Are You?</h1>
            <label for="year" id="yearborn" class="form-label">Year Born</label>
            <input type="range" class="form-range" id="year" name="year" value="0"/>
            <div class="form-label">You are: <span id="age"/></div>
        </div>

        <script>
            var d = new Date();
            var currentYear = d.getFullYear();
            var slider = document.getElementById("year");
            var yearBorn = document.getElementById("yearborn");
            var ageSpan = document.getElementById("age");

            fetchAndInstantiate('howold.wasm').then(function(instance) {
                slider.setAttribute("min", currentYear - 100);
                slider.setAttribute("max", currentYear);

                var updateLabels = function(val, age) {
                    yearBorn.innerText = "Year Born: " + val;
                    ageSpan.innerText = age;
                };

                var listener = function() {
                    var age = instance.exports.howOld(currentYear, slider.value);
                    updateLabels(slider.value, age);
                };

                slider.onchange = listener;
                slider.oninput = listener;
                slider.value = "1972";

                updateLabels(1972, 49);
            });
        </script>
```
```
</body>
</html>
```

渲染后的 HTML 应类似于图 5-2。

![](img/16b7990eb0dbf589db177bdbd2f4a189_102_0.png)

图 5-2. 我们用于计算某人年龄的 HTML 应用程序。

## 事情变得复杂

现在你已经看到了使用 C 与 WebAssembly 的基础，现实是，我们并没有使用该语言的太多部分。我向你展示了一个简单的示例，它将几个数字传递给一个仅返回单个数字的函数。这与我们迄今为止所做的事情并没有实质性不同。

更复杂的 C 程序将很难如此简单地映射到你所接触到的平台上。我们已经提到过关于“Hello, World!”程序的问题：浏览器中没有 printf() 函数可供程序使用。还有 C 程序的结构以及内存如何分配和清理的问题。将各种编译的软件文件链接在一起的过程在我们正在探索的这个世界中也是根本不同的。

好消息是，大多数这些问题都可以由工具和运行时平台处理。坏消息是，细节很快变得相当复杂。如果你以前从未做过任何 C 编程，将会有许多新想法。本书无法教你所有内容，但我会尝试突出该语言与 WebAssembly⁶ 之间的特定交互。

想象一个不带参数并返回数组总和的简单函数。示例 5-5 就有这样一个示例。忍耐一下，我暂时任意保持简单。在此代码中，我们没有参数，编译器可以知道数组需要多大，因为我们用前十个数字对其进行初始化。

## 示例 5-5. 一个简单的 C 函数

```
int addArray() {
    int retValue = 0;
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    for(int i = 0; i < 10; i++) {
        retValue += array[i];
    }

    return retValue;
}
```

如果我们尝试编译此程序，我们可能会收到警告，因为 clang 预计会有 main() 程序。记住，这是操作系统知道从哪里开始的方式，如我们在第 3 章中所讨论的。因为它找不到此名称的方法，所以无法将所有内容链接到独立运行时。

```
brian@tweezer ~/g/w/s/ch05> clang simple.c -o simple.o
Undefined symbols for architecture x86_64:
  "_main", referenced from:
      implicit entry/start for main executable
ld: symbol(s) not found for architecture x86_64
clang-11: error: linker command failed with exit code 1 (use -v
to see invocation)
```

