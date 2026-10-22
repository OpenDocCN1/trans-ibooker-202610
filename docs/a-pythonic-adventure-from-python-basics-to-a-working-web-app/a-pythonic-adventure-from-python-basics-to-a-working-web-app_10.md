## 完成菜单：一个真正的程序

## 本章内容

- Erik 和 Simon 在主程序中创建了加载和保存函数
- Simon 向主菜单添加了退出功能和 `get_order()` 函数
- Simon 解释了他为什么认为 Erik 刚刚创建了一个真正的程序
- 兄弟俩讨论未来的计划

“现在让我们来点实际的吧，” Simon 说。“昨天我们用示例订单和简单程序玩了一下。今天是时候把我们学到的东西用到真正的程序中了。”

“好啊，我们就这么做吧！” Erik 说。

“打开你的文件 `main_menu.py`，就是我们写主菜单的地方。我们应该添加几个函数来处理 JSON 文件。让我们先回顾一下我们要做什么，” Simon 说着拿出了昨天的图表。

![](img/f6295b544c84568800930d6ec95963ae_120_0.png)

“所以我们要写一个函数来从 JSON 文件加载订单列表。但首先它必须检查文件是否存在。如果不存在，我们创建一个空列表并从这个函数返回它。如果存在，我们就从中读取，将 JSON 转换为 Python 列表并返回该列表。”

“让我来帮你，” Simon 觉得对 Erik 来说有点 confusing。“通常，在处理文件的函数中我们会把文件名作为参数传递，” Simon 开始在文件底部、就在最后一行 main_menu() 之前写函数：

```
def load_orders(filename):
```

“现在我们要检查文件是否存在。有一个专门的函数可以做到这一点，我们可以在 os 模块中找到它。”

“‘os’ 是什么？” Erik 问。

“OS 代表 ‘operating system’。计算机中的操作系统管理所有文件和程序。它处理你的屏幕、键盘、扬声器和摄像头。在典型的计算机上，操作系统可以是 Windows、macOS 或 Linux。在我们的例子中，我们将询问操作系统这台计算机上是否存在这样名称的文件，” Simon 添加了一行：

```
def load_orders(filename):
    if os.path.exists(filename):
```

“看，我们在这里使用了 os 模块。这意味着我们必须像昨天在示例程序中导入 json 模块一样导入它。在这个程序中我们还没有导入 json 或 os 模块，所以让我们同时导入它们。”

Simon 将光标移到文件最开头并添加了两行：

```
import os
import json
```

他把光标返回到文件末尾的 load_orders() 函数并继续解释。“如果文件存在，我们打开它进行读取，使用 json.load() 函数从文件中读取到列表 orders 并返回列表。” 他向函数添加了三行。

```
def load_orders(filename):
    if os.path.exists(filename):
        f = open(filename, "r")
        orders = json.load(f)
        return orders
```

“如果不存在呢？我们只需创建一个空列表并返回它。”

```
def load_orders(filename):
    if os.path.exists(filename):
        f = open(filename, "r")
        orders = json.load(f)
        return orders
    else:
        orders = []
        return orders
```

“现在 load 函数准备好了！”他说并看着 Erik。

“我不认为我自己能写出来，” Erik 说。

“当然，第一次做的时候看起来很复杂。但看，你可以像读纯英语一样读它，不是吗？”

Erik 再次看向函数并尝试阅读它。“如果存在名为 ‘filename’ 的文件，则打开文件。将其保存在名为 ‘f’ 的对象中。然后从该 ‘f’ 文件中加载之前的订单到 ‘orders’。嗯…… 是的，我能读懂。” 他很惊讶。他现在能读懂 Python 了并且理解它！

“下一个函数更容易，” Simon 继续说。“我认为如果你看看我们练习示例订单的文件，你自己就能写出来。看，在两个 append() 操作之后有三行我们需要在这里。我们已经有一个名为 save_order() 的函数，它除了打印 ‘Saving order…’ 什么都不做。让我们用真正的函数替换它。我认为它应该叫做 save_orders()——复数，因为现在我们知道如何将订单列表保存到 JSON 文件中，对吧？”

Simon 写下了函数的开头：

```
def save_orders(orders, filename):
```

他解释道：“我们把订单列表作为第一个参数传递。然后传递我们想要存储它的文件名称。现在你可以添加我们昨天程序中的那三行。”

Erik 查看了 'dict_json.py' 文件并从中复制了三行。现在函数看起来像这样：

```
def save_orders(orders, filename):
    f = open(filename, "w")
    json.dump(orders, f, indent=4)
    return
```

“我们可以现在测试它吗？”他问 Simon。

“我们几乎准备好了，” Simon 回答。“看看我们文件的底部。现在我们只是调用 main_menu() 函数。还记得我昨天第二个图表吗？” 他拿出了那张画。

![](img/f6295b544c84568800930d6ec95963ae_123_0.png)

“这就是我们在 main_menu() 函数中要做的。我们只需要稍微修改它，以便为多位顾客服务并将他们的订单保存在列表 ‘orders’ 中。为此我们必须将该列表作为参数传递给 main_menu()，在那里有地方保存订单，因为它们被输入了。”

他将光标移到文件开头并向 main_menu() 定义添加了 orders 参数。

```
def main_menu(orders):
```

“现在，”他继续说，“每次顾客输入新订单时，它将被追加到 ‘orders’ 列表中。在我们添加列表作为参数之前，main_menu() 不知道在哪里添加新订单。现在我们可以使用 append() 方法并将其添加到 ‘orders’ 中。在顾客确认订单后，我们将其添加到订单列表中。我们不会在这里使用 save_order() 函数。我们会在你关闭程序时保存所有订单。” Simon 将 main_menu() 函数改为这样：

```
def main_menu(orders):
    while True:
        order = get_order()
        print("Check your order:")
        print_order(order)
        confirm = input("Confirm? Press Y to confirm, N to cancel: ")
        if confirm == "Y" or confirm == "y":
            orders.append(order)
            print("Thanks for your order:")
            print_order(order)
        else:
            continue
```

“我们还将主程序改为三个步骤：加载之前的订单、主菜单（获取新订单），并保存所有订单。” 他在文件底部添加了那三行。现在它看起来像这样：

```
orders = load_orders("orders.json")
main_menu(orders)
save_orders(orders, "orders.json")
```

“我现在可以试试吗？” Erik 问。

“当然，试试吧！” Simon 说。

Erik 运行程序，在第一个提示时输入了他的名字，然后选择了饮料的配料。当程序询问是否确认订单时，他输入了 “Y”。程序返回到 “What’s your name: ” 提示。

“好，” Erik 说，“它工作了。但我怎么能检查我的订单？它们保存在文件中了吗？”

“让我们检查一下，” Simon 说。

兄弟俩打开了 'orders.json' 文件，很惊讶地看到它仍然包含他们昨天实验的旧订单。连 Simon 也感到困惑。

“让我们看看，”他说。“我们打开文件，我们从中读取，我们获取订单……但我们从未向文件写入，因为我们仍在主菜单中接收订单！我们从未到达那个 save_orders() 函数！嗯……让我想想怎么修复它。”

Erik 微笑了。他的这个什么都知道的哥哥不知道该怎么办。

这只持续了一瞬间。

Simon 说：“我明白了。我们没有给用户提供退出主菜单的方法。我们一直询问用户的姓名，但如果我们想结束程序怎么办？”

“我看到你按 CONTROL C 来停止我的程序，” Erik 说。

“是的，我是这么做的，但那不是结束程序的正常方式。当我那样做时，程序被中断了。通常当你那样做时 Python 会给你一个错误信息。当程序被中断时意味着它不再做任何事情：它就在当前位置停止。它不会把我们的订单写入文件，也不会关闭文件。按 CONTROL C 是结束程序的糟糕方式。”

Simon 停顿了一下继续说：“我们应该给用户一个正常结束程序的方式。”

“像 Word 菜单里的 Exit 那样？”

“是的，就像那样。让我们告诉用户，如果他们想退出，就在询问姓名时输入 ‘X’ 并且只能输入 ‘X’。我们会有真实全名为 ‘X’ 的顾客的概率非常低。实际上几乎为零。所以让我们这样做：如果在 get_order() 函数中顾客



输入名字为 'X'，那么我们就不再问任何其他问题并返回一个空订单，像这样：order = {}。然后这个订单会传给 main_menu() 函数，它会判断：如果订单为空，就把订单保存到文件并退出。如果不为空，就把订单加入列表并继续工作。让我们画一个流程图。

![](img/f6295b544c84568800930d6ec95963ae_125_0.png)

“这就是我们更新后的 get_order() 函数。让我帮你写它吧，”Simon 说着拿过 Erik 的键盘开始编辑 get_order() 函数。

```
def get_order():
    order = {}
    name = input("Enter your name or enter 'X' to exit: ")
    if name == "X" or name == "x":
        return {}
    else:
        order["name"] = name
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    order["drink"] = menu(drinks, "Erik's drinks",
                          "Choose your drink: ")
    order["flavor"] = menu(flavors, "Erik's flavors",
                           "Choose your flavor: ")
    order["topping"] = menu(toppings, "Erik's toppings",
                            "Choose your topping: ")
    return order
```

“你为什么要同时检查 'X' 和 'x'？你告诉用户输入 'X'，所以他们应该输入大写的 'X'，对吧？”Erik 问。

“用户通常懒得按 Shift 键。他们可能会输入 'X' 或 'x'。所以我们应该同时检查。剩下的函数你应该能看懂——我只是按照我们一起画的流程图写的。”

“现在是主菜单，”Simon 继续说。“这里还有另一张流程图，”他开始画了起来。

![](img/f6295b544c84568800930d6ec95963ae_126_0.png)

“正如我们讨论的，如果 get_order() 返回一个空订单，我们就从主菜单退出。之后我们的程序会把订单保存到文件里。”Simon 把 main_menu() 函数编辑成如下所示：

```
def main_menu(orders):
    while True:
        order = get_order()
        if order == {}:
            print("You entered 'X', exiting...")
            return
        print("Check your order:")
        print_order(order)
        confirm = input(
"Confirm? Press Y to confirm, N to cancel, X to finish: ")
        if confirm == "Y" or confirm == "y":
            orders.append(order)
            print("Thanks for your order:")
            print_order(order)
        elif confirm == "X" or confirm == "x":
            return
        else:
            continue
```

## YOUR TURN    Edit the main menu function

编辑主菜单函数，类似 Simon 所做的那样。如果你需要帮助，本章的完整程序文本在这里

“就这样了，”Simon 说。“让我们测试一下。这次输入一个不同的名字，这样你就能看到它被添加到 JSON 文件中了。”

Erik 启动了程序。当程序询问名字时，他回答了 “Jason”。他输入了剩下的订单并输入 “Y” 确认订单。程序再次询问他的名字。

“现在让我们输入 'x' 看看是否能正常退出，”Simon 建议道。

Erik 输入了 'x' 并按下了 ENTER。

You entered 'X', exiting... 程序说着并返回了熟悉的 >>> Python 提示符。

“现在检查 orders.json 文件吧，”Simon 说。

Erik 启动了 TextEdit 并打开了 JSON 文件。在文件底部他找到了来自 'Jason' 的最新订单。

“成功了！”他说。“它把所有订单都保存到了文件里，现在我可以看到它们了！”

“是的，你可以，”Simon 说着笑了。他很高兴看到一个完整的、可工作的程序，它接收订单、将它们存储在文件里，而且是由他弟弟写的！

“恭喜你，Erik！”Simon 说。“我觉得现在你可以说你创建了一个真正的应用程序了。你看，它有输入和输出。它有数据结构和算法。它检查错误。它有数据存储。最重要的是：它能工作而且非常有用——它收集订单。我是认真的：这是一个好程序，我为你感到非常骄傲。”

和往常一样，Simon 画了一个他所说的真正程序的流程图。

![](img/f6295b544c84568800930d6ec95963ae_128_0.png)

“是的，我也喜欢我的程序，”Erik 说。“它做我想做的事，看起来也不错。它打印订单的方式几乎和我在星巴克看到的一样。是的，几乎……也许我可以加几行或几个星号让它更好看。我还有一些关于要为这个程序添加什么的想法。”

“你还想添加什么？”Simon 问。

“首先，我想把它做成一个 Web 应用程序。你知道的，有菜单和按钮。它应该在线上，这样我就可以带着我的 iPad 到处用。”

“好主意！”Simon 说。“我们下周开始做吧。我也有几个想法，”他笑了。

“你为什么笑？”Erik 问。

“我记得你第一次和我一起做完程序后以为已经完成了。”

“哈，是的，我也记得，”Erik 说。“当然，那时程序还不太完善。你还有什么其他想法？”

Simon 说：“我会在我们的数据结构中添加几样东西。例如，我们可以添加下单的日期和时间。这样我们就能看到每天或每月服务了多少客户。”

“是的，我觉得那样会很好，”Erik 同意道。

“然后，也许我们应该在订单输入后立即将其保存到数据存储中。这样即使程序出现故障并崩溃，我们也能保留所有之前的订单。”

“但你说那样会变慢，”Erik 回忆道。

“只是慢一点点。但值得——否则我们有丢失所有订单的风险。我在考虑使用数据库。”

“另外，”Simon 继续说，“我们需要像‘打印所有订单’和‘统计我们用了多少份香草口味’这样的函数。如果我们想让你的程序成为真正的商业应用。”

“当然，我想要，”Erik 说。“但首先我想把它做成 Web 应用程序并让它变得漂亮。”

“当然，我们下周可以开始做。”

## New things you have learned today

- **How to check if a file exists**
  我们使用了 Python 的 `os` 模块以及方法 `os.path.exists()`。你传入文件名，它会返回 `True` 或 `False`。
- **Pressing `CONRTOL C` is not the right way to finish a program**
  我们在想要停止行为异常的程序时使用它。好的程序应该始终给你一个正常结束的方式。
- **What a real program is**
  我们了解到真正的程序具有输入和输出、数据结构和算法、数据存储和错误检查。

## Code for this chapter

你可以在这里找到本章的代码：
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch09
