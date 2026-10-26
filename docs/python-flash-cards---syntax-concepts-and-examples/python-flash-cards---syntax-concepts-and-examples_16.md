## 包

![](img/b4d4158283a99b0f9a7bef8cfd6e5671_197_0.png)

使用 Python 编程的最大好处之一，就是已有大量现成的软件可供你在自己的工作中使用和构建。大多数包只需一两行代码即可安装（和卸载）。这些卡片将为你演示具体做法。

### 9.1 安装包

### 9.2 卸载 Python 包

## 

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

## 

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
