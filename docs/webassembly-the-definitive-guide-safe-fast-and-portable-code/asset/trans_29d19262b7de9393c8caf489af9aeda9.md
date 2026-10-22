

没问题。这是一个简单的修复。我们只需告诉 clang 编译代码但不链接它，使用 -c 选项即可：

```
brian@tweezer ~/g/w/s/ch05> clang -c simple.c -o simple.o
brian@tweezer ~/g/w/s/ch05> ls -laF simple.*
-rw-r--r--  1 brian  staff     170 Feb 19 15:27 simple.c
-rw-r--r--  1 brian  staff    1060 Feb 19 15:43 simple.o
```

这生成了一个包含函数定义的 object 文件。nm 命令⁷ 显示了编译文件中包含的内容：

```
brian@tweezer ~/g/w/s/ch05> nm -a simple.o
                U ___stack_chk_fail
                U ___stack_chk_guard
0000000000000000 T _addArray
                U _memcpy
00000000000000a0 s l___const.addArray.array
```

起初这可能令人困惑，但经过一些解释，思路应该足够清晰。我们的函数 addArray() 在 object 文件中被定义为一个 text 段符号。三个带有 U 符号类型的项目表示它们是未定义的。这些特定符号指的是出于安全原因自动链接的一些缓冲区溢出保护方法和一个用于从一个位置复制内存到另一个位置的函数。要使代码可执行，需要这些函数的定义，但这正是链接阶段和像 libc 这样的可重用库所提供的。

我们最终得到的是一个不完整的可执行文件，但我们函数的二进制表示是正确形成的。如果我们提供一个 main() 方法并链接可执行文件，我们就可以演示它的工作原理。在示例 5-6 中，我们的函数由驱动程序调用。

## 示例 5-6. 一个用于调用我们函数的 main() 方法。

```
#include <stdio.h>

extern int addArray();

int main() {
    int sum = addArray();
```
```
printf("The array sum is: %d\n", sum);
}
```

请注意，我们必须告诉编译器关于我们 addArray() 函数的定义，因为它未定义在此文件中。extern 关键字提供了一个承诺，即将存在一个名为此且不带参数并返回整数的东西可用。因此，将此函数的结果赋给一个名为 sum 的整数变量是可以的。然后它被传递给 printf() 函数，在那里被格式化为人类友好的输出消息，指示累积的和。

要构建可执行文件，我们编译 simplemain.c 和 simple.c 文件并将结果存储在名为 simplemain 的可执行文件中。因为我们没有包含 -c 选项，所以它会执行链接。这不再抱怨，因为我们确实提供了 main() 方法的定义。

```
brian@tweezer ~/g/w/s/ch05> clang simplemain.c simple.c -o simplemain
brian@tweezer ~/g/w/s/ch05> ls -laF simplemain
-rwxr-xr-x  1 brian  staff  49640 Feb 19 16:01 simplemain*
brian@tweezer ~/g/w/s/ch05> ./simplemain
The array sum is: 45
```

如果我们对最终的可执行文件使用 nm 程序，你会注意到我们这次提供了所需的一切。未定义的符号预计将在程序运行时由动态库提供。它们被排除在二进制之外以保持大小较小。

```
brian@tweezer ~/g/w/s/ch05> nm -a simplemain
                 U ___stack_chk_fail
                 U ___stack_chk_guard
0000000100008018 d __dyld_private
0000000100000000 T __mh_execute_header
0000000100003ea0 T _addArray
0000000100003e70 T _main
                 U _memcpy
                 U _printf
                 U dyld_stub_binder
```

现在我们有一个可工作的程序，让我们回到示例 5-5 中展示的函数。这之所以有效，是因为我们使用了字面语法来初始化数组。我们没有指定数组需要多大，因为编译器可以算出来。在内存中，它已分配了足够内存来容纳那么多整数。此分配是在栈上完成的，因此当我们返回函数时无需额外的清理。我们最终在内存中得到一个足够大的位置来存储我们要求和的数字，如图 5-3 所示。

![](img/16b7990eb0dbf589db177bdbd2f4a189_106_0.png)

图 5-3. C 数组只是一个存储我们数据的内存命名部分

如果我们告诉编译器它需要多大，但随后给它比那更多的数字会怎样？在示例 5-7 中，我们告诉编译器我们只期望数组中有五个整数，但随后给了它十个。在某些圈子里这被称为 blivet⁸。通过以下讨论，我希望向你展示编译器如何帮助你在编辑代码时通过对错误的反馈来得出更正确的解决方案。这发生在我们尝试运行代码之前，而在解释型语言中我们通常在那里发现问题。

## 示例 5-7. 我们函数的损坏版本。

```
int addArray() {
    int retValue = 0;
    int array[5] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    for(int i = 0; i < 10; i++) {
        retValue += array[i];
    }

    return retValue;
}
```

幸运的是，这也是编译器很容易检测到的。它会指出我们在胡闹并给出一个警告。

```
brian@tweezer ~/g/w/s/ch05> clang -c simple.c -o simple.o
    int array[5] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
```

1 warning generated.

如果我们想从函数返回一个数组会怎样？在示例 5-8 中我们尝试这样做，但很快就会发现它行不通。

## 示例 5-8. 尝试从函数返回数组但不成功的尝试。

```
int[] generateArray() {
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    return array;
}
```

即使我们所做的看起来合理，编译器再次告知我们我们做得不对。

```
brian@tweezer ~/g/w/s/ch05> clang -c simple2.c -o simple2.o
simple2.c:1:22: error: brackets are not allowed here; to declare an array, place the brackets after the identifier
int[] generateArray() {
  ~~
     ^
     []
simple2.c:1:20: error: function cannot return array type 'int []'
int[] generateArray() {
     ^
simple2.c:3:10: warning: incompatible pointer to integer conversion returning 'int [10]' from a function with result type 'int' [-Wint-conversion]
    return array;
           ^~~~~
simple2.c:3:10: warning: address of stack memory associated with local variable 'array' returned [-Wreturn-stack-address]
    return array;
           ^~~~~
2 warnings and 2 errors generated.
```

数组名在 C 中是特殊的变量。它们是内存中存储这些值的连续块地址的占位符。我们可以引入一个指向整数的指针并将其赋值为数组开头的地址。为了访问该位置的值，我们必须使用解引用运算符，*。

## 示例 5-9. 使用指向数组的指针

```
#include <stdio.h>

void generateArray() {
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int * a = array;
    printf("a is %p\n", a);
    printf("The first value is: %d\n", *(a));
    printf("The second value is: %d\n", *(a + 1));
    printf("The third value is: %d\n", *(a + 2));
}

int main() {
    generateArray();
}
```

数组中的第一个值位于数组开头，因此我们可以用 *a 访问它。第二个整数在下一个内存地址上，所以我们在解引用之前先将一加到数组基址上。第三个值在两个位置之后。

编译我们的程序并运行它会显示我们期望的输出。你的 a 的地址值可能不同，但它应该看起来相似。

```
brian@tweezer ~/g/w/s/ch05> clang simple3.c -o simple3
brian@tweezer ~/g/w/s/ch05> ./simple3
a is 0x7ffee f3a9720
The first value is: 0
The second value is: 1
The third value is: 2
```

编译器对示例 5-8 中的代码大喊大叫的原因是我们不能像尝试的那样返回一个数组。相反，你必须返回一个指针。

我们再次尝试在示例 5-10 中返回我们的数组。

