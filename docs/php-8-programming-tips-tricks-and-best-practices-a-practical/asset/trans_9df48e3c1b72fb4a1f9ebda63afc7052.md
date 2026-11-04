
```
object(FFI\CData:char[3][3])#2 (3) {
    [0]=> object(FFI\CData:char[3])#3 (3) {
        [0]=> string(1) "A"
        [1]=> string(1) "B"
        [2]=> string(1) "C"
    }
    [1]=> object(FFI\CData:char[3])#1 (3) {
        [0]=> string(1) "D"
        [1]=> string(1) "E"
        [2]=> string(1) "F"
    }
    [2]=> object(FFI\CData:char[3])#4 (3) {
        [0]=> string(1) "G"
        [1]=> string(1) "H"
        [2]=> string(1) "I"
    }
}
```

从输出中首先需要注意的重要一点是，所有索引都是整数。从输出中得到的第二个结论是，这显然不是原生的 PHP 数组。var_dump() 显示给我们，每个数组元素都是一个 FFI\CData 实例。此外，请注意 C 语言字符串被当作数组处理。

由于该数组的类型为 char，我们可以使用 FFI::string() 来显示其中一行。以下是产生 ABC 响应的命令：

```
echo FFI::string($arr[0], 3);
```

任何尝试将 FFI\CData 实例提供给接受数组作为参数的 PHP 函数都注定会失败，即使它被定义为数组类型。在下面的代码片段中，请注意如果将此命令添加到前面的代码块中会产生的输出：

```
echo implode(',', $arr);
```

正如接下来所示的输出，你可以看到，由于数据类型不是数组，implode() 会引发致命错误。以下是 resulting 输出：

```
PHP Fatal error: Uncaught TypeError: implode(): Argument #2 ($array) must be of type ?array, FFI\CData given in /repo/ch04/php8_ffi_array.php:25
```

你现在已经知道如何创建和使用 FFI\CType 实例。现在让我们把注意力转向创建 FFI 实例。

## 创建和使用 FFI 实例

如本章引言中所述，FFI 扩展促进了快速原型设计。因此，使用 FFI 扩展，你可以一次开发一个设计用于新扩展的 C 函数，并在 PHP 应用程序内部立即对其进行测试。

> **重要提示**
> FFI 扩展不会编译 C 代码。为了使用 FFI 扩展中的 C 函数，你必须首先使用 C 编译器将 C 代码编译成共享库。你将在本章最后一节 *在应用程序中使用 FFI* 中学习如何执行此操作。

为了在 PHP 和原生 C 库函数调用之间建立桥梁，你需要创建一个 FFI 实例。FFI 扩展需要你提供一个 C 定义，该定义定义了 C 函数签名以及计划使用的 C 库。FFI::cdef() 和 FFI::scope() 都可以直接用于创建 FFI 实例。

以下示例使用 FFI::cdef() 来绑定两个原生 C 库函数。以下是发生的情况：

- 1. 第一个原生方法 srand() 用于为随机化序列设置种子。rand()，另一个原生 C 函数，调用序列中的下一个数字。$key 变量保存随机化的最终结果。$size 表示要调用的随机数个数。代码在以下片段中说明：
- 2. 然后我们通过调用 cdef() 创建 FFI 实例，并在字符串 $code 中标识原生 C 函数，该字符串取自 libc.so.6 原生 C 库，如下所示：

// /repo/ch04/php8_ffi_cdef.php
$key = '';
$size = 4;
```

```
$code = <<<EOT
    void srand (unsigned int seed);
    int rand (void);
EOT;
$ffi = FFI::cdef($code, 'libc.so.6');
```

3. 然后我们通过调用 srand() 为随机化设置种子。然后，在循环中，我们调用 rand() 原生 C 库函数来生成随机数。我们使用 sprintf() 原生 PHP 函数将结果整数转换为十六进制，其输出附加到 $key，然后输出。代码如下所示：

```php
$ffi->srand(random_int(0, 999));
for ($x = 0; $x < $size; $x++)
    $key .= sprintf('%x', $ffi->rand());
echo $key
```

以下是前面代码片段的输出。请注意，结果值可用作随机密钥：

```
```
root@php8_tips_php8 [/repo/ch04 ]# php php8_ffi_cdef.php
23f306d51227432e7d8d921763b7eedf
```
```

在输出中，你会看到一系列连接的随机整数转换为十六进制。请注意，每次调用脚本时，结果值都会发生变化。

**提示**
对于真正的随机化，使用 random_int() 原生 PHP 函数可能更好。openssl 扩展中也包含一些优秀的密钥生成函数。此处所示的示例主要旨在让你熟悉 FFI 扩展的使用。

**重要提示**
FFI 扩展还包括两个额外的创建方法：
FFI::load() 和 FFI::scope()。FFI::load() 用于在预加载过程中直接从 C 头文件（*.h）加载 C 函数定义。FFI::scope() 使预加载的 C 函数可通过 FFI 扩展使用。有关预加载的更多信息，请在此处查看 FFI 文档中的完整预加载示例：https://www.php.net/manual/en/ffi.examples-complete.php。

现在让我们看看 FFI 扩展中用于比较原生 C 数据类型的函数。

## 使用 FFI 比较数据

需要记住的是，当你使用 FFI 扩展创建 C 语言数据结构时，它存在于你的 PHP 应用程序之外。如你在前面的示例中看到的（参见 创建和使用 FFI\CType 实例 部分），PHP 在一定程度上可以与 C 数据交互。然而，对于比较目的，最好使用 FFI::memcmp()，因为原生 PHP 函数可能会返回不一致的结果。

FFI 扩展中可用的两个比较函数在此总结于表 4.2 中：

| FFI 方法 | 返回 | 说明 |
|---|---|---|
| isNull() | bool | 如果 FFI\CData 实例为空指针则返回 TRUE。 |
| memcmp() | int | 其操作方式与 PHP strcmp() 函数或 <=> 运算符非常相似。如果第一个 FFI\CData 实例小于第二个，则返回小于 0 的值。如果两者相等，则返回 0，如果第二个参数的值大于第一个，则返回大于 0 的值。 |

表 4.2 – FFI 类比较方法摘要

可以使用 FFI::isNull() 来确定 FFI\CData 实例是否为空。更有趣的是 FFI::memcmp()。尽管此函数的操作方式与 spaceship 运算符（<=>）相同，但它接受第三个参数，表示你希望包含在比较中的字节数。以下示例说明了此用法：

- 1. 我们首先定义一组四个变量，表示可以包含多达六个字符的 FFI\CData 实例，并用示例数据填充这些实例，如下所示：

// /repo/ch04/php8_ffi_memcmp.php
$a = FFI::new("char[6]");
$b = FFI::new("char[6]");
$c = FFI::new("char[6]");
$d = FFI::new("char[6]");
```

2. 回想一下，C 语言将字符数据视为数组，因此我们不能直接分配字符串，即使使用 cdata 属性。因此，我们需要定义一个匿名函数，用字母填充这些实例。我们使用以下代码来实现此目的：

```
$populate = function ($cdata, $start, $offset, $num) {
    for ($x = 0; $x < $num; $x++)
        $cdata[$x + $offset] = chr($x + $offset +
                                    $start);
    return $cdata;
};
```

3. 接下来，我们使用该函数用不同的字母集填充四个 FFI\CData 实例，如下所示：

```
$a = $populate($a, 65, 0, 6);
$b = $populate($b, 65, 0, 3);
$b = $populate($b, 85, 3, 3);
$c = $populate($c, 71, 0, 6);
$d = $populate($d, 71, 0, 6);
```

4. 现在我们可以使用 FFI::string() 方法来显示迄今为止的内容，如下所示：

```
$patt = "%2s : %6s\n";
printf($patt, '$a', FFI::string($a, 6));
printf($patt, '$b', FFI::string($b, 6));
printf($patt, '$c', FFI::string($c, 6));
printf($patt, '$d', FFI::string($d, 6));
```

5. 以下是 printf() 语句的输出：

```
$a : ABCDEF
$b : ABCXYZ
$c : GHIJKL
$d : GHIJKL
```
