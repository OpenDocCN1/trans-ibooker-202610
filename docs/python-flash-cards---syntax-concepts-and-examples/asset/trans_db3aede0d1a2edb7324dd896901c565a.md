
下面是对卡片 6.8 中 `area_rect()` 函数的一个测试：

```
import unittest
import area_functions as af

class AFTestCase(unittest.TestCase):
    """Tests for area_functions.py."""

    def test_rect_area(self):
        """Test a simple rectangle."""
        area = af.area_rect(3, 4)
        self.assertEqual(area, 12)

if __name__ == '__main__':
    unittest.main()
```

你需要导入 `unittest` 和所要测试的模块。测试类名称通常以 `TestCase` 结尾；前缀用于表明它们测试的内容。测试类必须是 `unittest.TestCase` 的子类。

每个测试都是一个方法。所有以 `test_` 开头的方法都会在测试运行时执行。`test_area_rect()` 方法使用矩形尺寸调用 `area_rect()`，然后检查返回的面积是否正确。

## 运行通过的测试

- 如何为函数运行测试？
- 通过的测试是什么样的？

要为函数运行测试，请运行包含该函数测试的文件：

```
$ python test_area_functions.py
.
-----------------------------
Ran 1 test in 0.000s
OK
```

对于通过的测试，Python 会打印一个点。只运行一个测试的结果并不是很有帮助，但较长的测试用例输出要有用得多。失败的测试会显示 `E` 或 `F`，分别表示错误或断言失败。

测试完成后，会显示一个摘要，指明测试的数量及其运行时间。`OK` 消息表示所有测试均已通过。

## 运行失败的测试

- 失败的测试是什么样的？
- 你能从失败的测试中学到什么？

如果代码产生 Python 错误或断言失败，测试就会失败。

下面是 `test_area_functions.py` 中的一个新测试，用于检查负输入是否返回 `None`：

```
def test_negative_inputs(self):
    """Test negative inputs."""
    area = af.area_rect(-3, 4)
    self.assertEqual(area, None)
```

这个测试会失败；以下是输出：

```
$ python test_area_functions.py
F.
====================
FAIL: test_negative_inputs
Test negative inputs for area_rect().
--------------------
Traceback (most recent call last):
  File "test_area_functions.py", line 15:
    self.assertEqual(area, None)
AssertionError: -12 != None
--------------------
Ran 2 tests in 0.000s
FAILED (failures=1)
```

失败的测试会显示 `F`：输出会显示失败的测试，并打印其文档字符串、失败的断言语句，以及断言语句中的值。最后的摘要会显示测试数量、运行时间和失败项。

## 修复失败的测试

- 如何修复失败的测试？

## `setUp()` 方法

- `setUp()` 在测试用例中有什么作用？
- 如何利用 `setUp()` 让测试代码更高效？

将初始测试需求放在 `setUp()` 方法中，使其在每个测试之前自动运行，从而让代码更高效、更一致。

在示例测试用例（卡片 8.2 和 8.4）中，我们在每个测试函数中都定义了宽和高。相反，我们可以改为在 `setUp()` 中定义宽和高，然后在每个测试函数中使用这些值：

```
class AFTestCase(unittest.TestCase):
    """Tests for area_functions.py."""

    def setUp(self):
        """Create widths and lengths for all tests."""
        self.width = 3
        self.length = 4
```

然后，测试方法将如下所示：

```
def test_rect_area(self):
    """Test a simple rectangle."""
    area = af.area_rect(
        self.width, self.length)
    self.assertEqual(area, 12)
```

这个 `setUp()` 函数很简短。在处理复杂输入值的实际测试中，`setUp()` 函数能让测试代码更高效，并确保众多测试之间的一致性。

## 包

![](img/b4d4158283a99b0f9a7bef8cfd6e5671_197_0.png)

使用 Python 编程的最大好处之一，就是已有大量现成的软件可供你在自己的工作中使用和构建。大多数包只需一两行代码即可安装（和卸载）。这些卡片将为你演示具体做法。

### 9.1 安装包

### 9.2 卸载 Python 包

## 安装包

- 什么是包？
- 什么是库？
- 什么是 pip？
- 如何安装 Python 包？

包是包含模块的目录。为共享而组织的包通常称为库。安装包后，你就能使用其全部函数和代码。

`pip` 工具用于安装 Python 包，并已包含在当前所有 Python 版本中。要使用 `pip` 安装包，请将 `pip` 作为模块运行，并附上包名：

```
$ python -m pip install --user requests
Collecting requests
--snip--
Installing collected packages: requests
Successfully installed requests-2.18.4
```

`-m` 标志将 `pip` 作为模块运行。`--user` 标志仅将包安装给当前用户。`requests` 参数是要安装的包名。你将会看到包的下载过程、所需依赖项的输出、安装确认以及版本号。

## 卸载 Python 包

- 如何卸载 Python 包？

你可以使用 `pip` 卸载任何包：

```
$ python -m pip uninstall requests
Uninstalling requests-2.18.4:
--snip--
Proceed (y/n)? y
    Successfully uninstalled requests-2.18.4
```

你会看到将要卸载的相关包的摘要；输入 `y` 继续。随后会有消息确认卸载。无需使用 `--user` 标志。
