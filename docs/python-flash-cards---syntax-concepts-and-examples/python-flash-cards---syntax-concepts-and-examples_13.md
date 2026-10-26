## 模块

- 什么是模块？
- 如何创建模块？

模块是包含多个函数或类的 Python 文件。通过将这些函数存储在单独的文件中，你可以在程序中调用它们，而不必每次都输入完整的函数代码。

这个 area_functions.py 模块包含两个函数：

```python
"""A set of functions for calculating areas."""

def area_rect(w, h):
    """Calculate the area of a rectangle."""
    --snip--

def area_circle(r):
    """Calculate the area of a circle."""
    --snip--
```

这个文件中没有任何函数调用，因此这段代码在主编程序调用它之前不会运行。当你为模块中的函数添加功能或进行改进时，这些更改会立即可供所有使用它们的程序使用。

### 导入模块

导入后，通过提供模块名称、一个点和函数名称来使用任何模块函数：

```python
import area_functions
my_area = area_functions.area_rect(4, 5)
print(my_area)
```

这段代码的输出与包含所有函数代码时的输出相同：

```text
20
```

使用 as 为模块指定别名，以便使用更短的函数调用。这里我们用 af 代替 area_functions：

```python
import area_functions as af
my_area = af.area_rect(4, 5)
print(my_area)
```

### 导入特定函数

- 如何从一个模块中导入一个函数？
- 如何从一个模块中导入多个单独的函数？
