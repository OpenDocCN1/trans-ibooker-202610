

# 使用包

为了测试包的行为是否符合预期，我们可以创建一个满足 `Unmarshaller` 接口的自定义类型。该类型在解码时将字符串转换为大写：

```
type UpperString string

func (u *UpperString) UnmarshalProp(b []byte) error {
    *u = UpperString(strings.ToUpper(string(b)))
    return nil
}
```

现在我们可以将该类型用作结构体字段，并验证它在 `decode` 操作中是否被正确转换：

```
func main() {
    r := strings.NewReader(
        "\n# comment, ignore\nkey1: 10.5\nkey2: some string" +
            "\nkey3: 42\nkey4: false\nspecial: another string\n")
    var v struct {
        Key1 float32
        Key2 string
        Key3 uint64
        Key4 bool
        Key5 UpperString `prop:"special"`
        key6 int
    }
    if err := prop.NewDecoder(r).Decode(&v); err != nil {
        log.Fatal(r)
    }
    log.Printf("%+v", v)
}
```

# 总结

在本章中，我们详细回顾了 Go 语言中接口的内存模型，重点强调了接口始终包含一个具体类型。我们利用这些信息深入了解了类型转换，并理解了当接口被转换为另一个接口时会发生什么。

然后，我们介绍了反射的基本机制，从 `Type` 和 `Value` 开始，它们是该包的两个主要类型。它们分别代表变量的类型和值。`Value` 允许你读取变量的内容，如果变量是可寻址的，还允许写入它。为了使变量可寻址，需要通过其地址访问该变量，例如使用指针。

我们还看到了如何使用反射处理复杂数据类型，了解了如何访问结构体字段的值。结构体的数据类型可用于获取关于字段的元数据，包括名称和标签，这些在编码包和其他第三方库中被大量使用。

我们了解了如何创建和操作映射，包括添加、设置和删除值。对于切片，我们看到了如何编辑其值以及如何执行追加操作。我们还展示了如何使用通道发送和接收数据，甚至如何以静态类型编程相同的方式使用 `select` 语句。

最后，我们列出了标准库中使用反射的地方，并快速分析了其计算成本。我们以一些关于如何以及何时在库或你正在编写的任何应用程序中使用反射的提示作为本章的结尾。

下一章是本书的最后一章，它解释了如何使用 CGO 在 Go 中利用现有的 C 库。

# 问题

1.  Go 中接口的内存表示是什么？
2.  当接口类型转换为另一个接口类型时会发生什么？
3.  反射中的 `Value`、`Type` 和 `Kind` 是什么？
4.  如果一个值是可寻址的意味着什么？
5.  为什么结构体字段标签在 Go 中很重要？
6.  反射的一般权衡是什么？
7.  你能描述一种使用反射的良好方法吗？

# 使用 CGO

本章将向你介绍 CGO，这是一个用于 C 语言的 Go 运行时。它使得从 Go 应用程序调用 C 代码成为可能，由于 C 语言拥有大量可用的库，这意味着它们可以在 Go 中被利用。

本章将涵盖以下主题：

*   从 C 和 Go 使用 CGO
*   理解类型差异

## 技术要求

本章要求已安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 [第 3 章](#602a92d5-25f7-46b8-83d4-10c6af1c6750.xhtml) *Go 概览*。

此外，它要求你的机器上已安装 GCC 编译器。这可以在你的 Unix 机器上使用包管理器轻松完成。对于 Ubuntu，命令如下：

```
sudo apt install gcc
```

## CGO 简介

CGO 是使在 Go 应用程序中运行 C 代码成为可能的工具。自 2009 年 Go 达到 1.0 版本以来，此功能就已存在，它允许我们在当时可用的标准库外包比今天少得多的情况下使用现有的 C 库。

C 代码通过 `C` 伪包访问，并通过包名后跟标识符来访问和调用，例如 `C.print`。

`import` 声明前面有一系列特殊注释，指定应用程序应导入哪个 C 源文件：

```
package example

// #include <stdio.h>
import "C"
```

该语句也可以是多行注释，可以包含更多的 `include` 指令，如前面示例中的那样，甚至可以直接包含实际的 C 代码：

```
package example

/*
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>

void someFunction(char* s) {
    printf("%s\n", s);
}
*/
import "C"
```

重要的是避免在 C 注释和 `import` 语句之间有空行，否则库和代码将不会被 CGO 导入到应用程序中。

## 从 Go 调用 C 代码

要使用由我们或他人编写的现有 C 代码，我们需要从 Go 调用 C。让我们执行一个快速、完整的示例，仅使用 C 功能将字符串打印到标准输出：

```
package main

/*
#include <stdio.h>
#include <stdlib.h>

void customPrint(char* s) {
    printf("%s\n", s);
}
*/
import "C"

import "unsafe"

func main() {
    s := C.CString(`Printing to stdout with CGO
        Using <stdio.h> and <stdlib.h>`)
    defer C.free(unsafe.Pointer(s))
    C.customPrint(s)
}
```

我们在这里导入了两个 C 核心库，它们分别是：

*   `stdio.h`：包含输入和输出方法。我们使用的是 `printf`。
*   `stdlib.h`：包含通用函数，包括内存管理。

查看前面的代码，我们注意到我们要打印的变量不是普通的 Go `string`，而是由 `C.CString` 函数获得的，该函数接受一个字符串并返回一个 `char` 切片，因为这是字符串在 C 中的处理方式。该函数定义如下：

```
func C.CString(string) *C.char
```

我们观察到的第二件事是我们推迟了对 `C.free` 的调用，传递我们定义的 `s` 变量，但将其转换为不同的类型。此函数调用至关重要，因为该语言没有垃圾回收，为了释放已使用的内存，应用程序需要专门调用 C 的 `free` 函数。该函数接收一个通用指针，在 Go 中由 `unsafe.Pointer` 类型表示。根据 Go 文档，以下内容适用：

"任何类型的指针值都可以转换为 Pointer。"

