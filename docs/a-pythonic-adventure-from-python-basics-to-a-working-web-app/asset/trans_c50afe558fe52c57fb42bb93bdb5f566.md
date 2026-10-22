

![](img/f6295b544c84568800930d6ec95963ae_35_0.png)

![](img/f6295b544c84568800930d6ec95963ae_35_1.png)

"现在我该做什么？" Erik 问。他觉得自己理解了 Simon 的解释，但仍然被这个不便所困扰。

"我看你很恼火，" Simon 说。"别担心，你很快就会习惯的。而且你会像所有真正的程序员一样，从零开始计数，" Simon 微笑。"现在你只需在方括号中的每个索引减去一。但要小心：你必须在将输入转换为整数之后再加上那个 - 1，而不是之前。像这样：`drinks[int(drink) - 1]` 。"

Erik 修复了他的代码，现在它看起来像这样：

```
drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]

print("Erik's Coffee Shop drinks")
print("--------------------------")
i = 1
for d in drinks:
    print(i, d)
    i = i + 1
drink = input("Choose your drink: ")

print("Erik's Coffee Shop flavors")
print("--------------------------")
i = 1
for f in flavors:
    print(i, f)
    i = i + 1
flavor = input("Choose your flavor: ")

print("Erik's Coffee Shop toppings")
print("--------------------------")
i = 1
for t in toppings:
    print(i, t)
    i = i + 1
topping = input("Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drinks[int(drink) - 1])
print("Flavor: ", flavors[int(flavor) - 1])
print("Topping: ", toppings[int(topping) - 1])
print("Thanks for your order!")
```

## YOUR TURN   Modify your program to print the order

修改你的程序，模仿 Erik 刚才的做法并打印订单。不要忘记将输入字符串转换为数字。不要忘记从每个数字中减去一（1）——列表索引从零开始，记住？

他运行了程序，输入了 2、1、1，最后得到了他想要的结果：

```
Here is your order:
Main product:  chocolate
Flavor:  caramel
Topping:  chocolate
Thanks for your order!
>>>
```

"酷！它工作了！" Erik 非常高兴。"我喜欢我的咖啡店程序！我们做完了吗？"

"快了，" Simon 回答。"看，你几乎完全重复写了三次同样的代码。"

"这有什么问题？"

"想象一下你想在代码中更改某些东西。例如，改变打印菜单项的方式。你必须在所有三个地方都更改它。或者如果你决定添加其他菜单列表，就会有更多地方需要改。想象一下你想在咖啡店中添加甜点。这意味着你必须再复制一次这段代码。如果你在代码中犯了错误呢？程序员称之为 *bug*（我稍后会告诉你为什么）。那么你就必须在四个地方修复那个 bug，反复重复自己。程序员喜欢 DRY 原则：不要重复自己。"

"但我不知道该怎么做，" Erik 很困惑。"如果我有 *三个* 菜单列表，我必须打印它们三次。而且我必须三次向用户请求输入。"

"我们可以在这里使用一个函数，" Simon 解释道。"还记得我们开始使用 `print()` 函数时，我告诉过你，对于我们想要反复执行的操作，我们使用函数。到目前为止我们使用的是别人编写的函数。现在我们将创建自己的函数并使用它。"

"这很酷，我喜欢，" Erik 说。

"太好了，我们明天再做吧。我想今天已经做得够多了。你做得很好，Erik，" Simon 确实很高兴看到他的弟弟在进步。"让我们回顾一下今天我们学到了什么。第一件事是什么？"

"首先，我们创建了 *列表*，" Erik 说。"我们把所有的饮料、口味和配料都放在列表中。"

"很好，接下来呢？"

"然后我们使用循环打印列表。然后我们在每个饮料或口味旁边打印数字。"

"是的，完全正确，" Simon 确认。"继续，接下来是什么？"

"然后我尝试从列表中打印饮料，但 Python 给了我一个错误。然后你向我解释了数字是如何存储在计算机内存中的。然后我们转换了数字，我再次尝试打印我的订单。而 *就因为你没有告诉我索引从零开始*，（Erik 没忘！）"我的订单打印出了所有错误的东西！"

"请原谅我，" Simon 微笑。"但现在你会记得更牢，我相信！"

"最后我修复了它，现在它工作得很好！" Erik 总结道。

"做得好！" Simon 给了 Erik 一个竖大拇指。"我们明天继续，写我们的第一个函数。

## New things you have learned today

- **List**
  Python 中的一组项目。你可以在列表中拥有字符串或数字，甚至是它们的混合。
- **List index**
  我们可以用它从列表中检索项目的数字。列表中的索引总是从零开始，并为每个下一个元素增加一：0、1、2、3 等。
- **Numbers and strings**
  这些是 Python 中不同 *类型* 的变量。当你在屏幕上打印某些内容，或从键盘获取输入时，你总是使用字符串。当你想对从用户那里接收到的数字执行任何数学运算时，你必须先将它们从字符串转换为数字。

## Code for this chapter

你可以在这里找到本章的代码：
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch02

## Functions: Don’t repeat yourself!

## In this chapter

- Simon 解释如何避免重复自己（以及为什么）
- Erik 编写他的第一个 Python 函数
- Erik 开始使用 Python 的交互式工具
- Erik 改进他的函数，使收据看起来更专业

"我们昨天停在哪里？" Simon 第二天问 Erik。

"你说我不应该重复自己。而且你说我们今天要写自己的函数。"

"对！首先，告诉我你目前对函数了解多少。"

"我们已经使用了一些函数了，" Erik 开始回答。"我们使用了 print() 和 input()。你说有人为我们写了它们，所以我们可以直接使用。函数可以使用参数。我们只需把它们放在括号之间，函数就会对参数做一些事情，比如打印它们。"

"一切都对！你是个好学生！" Simon 微笑。"函数的重要之处在于它们可以用不同的参数做同样的事情。所以如果你发现自己在做同样的事情多次，你应该看看是否可以把它变成一个函数。为了决定，你应该查看重复的代码并问自己哪些部分相同，哪些不同。昨天的脚本在哪里？"

"在这里，" Erik 在编辑器中打开了 menu.py 文件。

```
drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]

print("Erik's Coffee Shop drinks")
print("----------------------------")
i = 1
for d in drinks:
    print(i, d)
    i = i + 1
drink = input("Choose your drink: ")

print("Erik's Coffee Shop flavors")
print("----------------------------")
i = 1
for f in flavors:
    print(i, f)
    i = i + 1
flavor = input("Choose your flavor: ")

print("Erik's Coffee Shop toppings")
print("----------------------------")
i = 1
for t in toppings:
    print(i, t)
    i = i + 1
topping = input("Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drinks[int(drink) - 1])
print("Flavor: ", flavors[int(flavor) - 1])
print("Topping: ", toppings[int(topping) - 1])
print("Thanks for your order!")
```

"看：这里有什么在重复？" Simon 问并开始和 Erik 一起画出代码的图表。

![](img/f6295b544c84568800930d6ec95963ae_40_0.png)

"循环。开头的 print() 和末尾的 input()。"

"这三种情况下有什么不同？"

"input() 的提示不同，" Erik 回答。"标题也略有不同。"

"而且你还在对不同的列表进行 for 循环，比如 drinks、flavors 和 toppings，对吧？" Simon 决定帮助 Erik。"所以我们将把不同的东西作为参数传递。在我们的例子中，它们将是选项列表、菜单标题和输入提示。"

"让我们在编辑器中新建一个文件，命名为 menu_function.py，并在其中编写我们的函数。"

