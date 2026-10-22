

![](img/f6295b544c84568800930d6ec95963ae_98_0.png)

"我们先从 get_name 开始。这里我们做什么？" Simon 问。

"我们只要问‘你的名字是什么？’，" Erik 回答。

"对，然后呢？"

"然后我们把它保存在一个变量里，比如 name。"

"差不多了，" Simon 说。"记住，我们决定 order 是一个字典。我们会把与该订单相关的所有内容都保存在这个字典里。例如，要保存顾客的名字，而不是 name = 'Erik'，我们应该写 order['name'] = 'Erik'。只是用函数 input() 代替 'Erik'，就像你在第一个程序里做的那样。"

"让我试试，" Erik 说并写下了这个函数：

```
def get_order():
    order['name'] = input("What's your name: ")
    return {}
```

"现在试试，" Simon 说。

Erik 点击了 Run，程序询问他的名字，他输入了 'Erik'。但随后他得到了几行错误信息。

```
What's your name: Erik
Traceback (most recent call last):
  File "/home/erik/mu_code/main_menu.py", line 41, in <module>
    main_menu()
  File "/home/erik/mu_code/main_menu.py", line 3, in main_menu
    order = get_order()
  File "/home/erik/mu_code/main_menu.py", line 27, in get_order
    order['name'] = input("What's your name: ")
NameError: name 'order' is not defined
>>>
```

"这是什么？？？" 他问 Simon。

"看，Python 告诉你问题发生在哪里。读最后一行。"

"Name 'order' is not defined，" Erik 读到。

"很简单，" Simon 解释道。"你试图往字典里放东西，但你还没创建它。很容易修复。让我们创建一个空字典。还记得我们用大括号来做这个吗？就在 input () 那行之前写 order = {} 即可。"

Erik 把他的函数改成了这样：

```
def get_order():
    order = {}
    order['name'] = input("What's your name: ")
    return {}
```

然后再次运行。这次它没有给他任何错误。

```
What's your name: Erik
Check your order:
{}
Confirm? Press Y to confirm, N to cancel: y
Saving order...
Thanks for your order:
{}
What's your name:
```

"现在好点了，" 他说。

"是的，好点了，但看：它仍然打印一个空订单。你创建了一个订单，甚至输入了你的名字，但你的函数返回的是一个空字典。看这一行：return {}？"

"但这是你写的！" Erik 确信这不是他的错。

"是的，我就是这样写来测试主菜单函数的。但现在我们必须返回实际的订单字典。把它改成 return order 然后看看它是否会打印你的名字。"

Erik 把函数改成了这样：

```
def get_order():
    order = {}
    order['name'] = input("What's your name: ")
    return order
```

然后再次运行。这次他看到：

```
What's your name: Erik
Check your order:
{'name': 'Erik'}
Confirm? Press Y to confirm, N to cancel:
```

"是的，它现在打印我的名字了！"

"恭喜！" Simon 说。"现在你知道如何使用字典了！"

## YOUR TURN   Create your get_order() function
开始在 main_menu.py 文件中编写你自己的 get_order() 函数。添加第一个输入来获取顾客的名字。通过运行主菜单程序来测试它。

### What are your choices?

"我们继续吧，" Simon 继续说。"我们现在必须添加你的 menu() 函数来填写饮料和口味。但我们也需要 read_menu() 函数来从文件读取你的菜单。复制这两个函数（menu() 和 read_menu() 来自 menu_files.py 文件）并把它们粘贴到这里的 main_menu.py 中。把它们粘贴在 def get_order() : 行之前。"

"如果我把它粘贴在这行之后会怎样？" Erik 想知道为什么他哥哥给他下这样的严格命令。

"那样就不会工作，" Simon 给了他一个简单的答案并笑了。"好吧，如果你真的想知道：我们将要在 get_order() 函数中使用这两个函数。首先我们要从文件读取菜单内容：你的饮料、你的口味、你的配料。然后我们调用 menu() 函数三次来获取顾客的选择。在我们可以使用这些函数之前，我们应该先定义它们。换句话说，我们应该告诉 Python 存在这样的函数以及它们做什么。这就是为什么它们需要在 def get_order() : 行之前粘贴。" 顺便说一下，这就是我们使用 def 这个词来开始一个函数的原因——我们定义它。"

"好的，" Erik 说并开始复制函数。过了一会儿，他的 main_menu.py 文件看起来像这样：

# Listing 7.1 main_menu.py

```
def main_menu():
    while True:
        order = get_order()
        print("Check your order:")
        print_order(order)
        confirm = input("Confirm? Press Y to confirm, N to cancel: ")
        if confirm == "Y" or confirm == "y":
            save_order(order)
            print("Thanks for your order:")
            print_order(order)
        else:
            continue

def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    print(title)
    print(len(title) * "-_")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    while True:
        choice = input(prompt)
        allowed_answers = []
        for a in range(1, len(choices) + 1):
            allowed_answers.append(str(a))

        allowed_answers.append("X")
        allowed_answers.append("x")

        if choice in allowed_answers:
            if choice == "X" or choice == "x":
                answer = ""
                break
            else:
                answer = choices[int(choice) - 1]
                break
        else:
            print("Enter number from 1 to ", len(choices))
            answer = ""
    return answer

def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)
    return result
# . . .
# No changes in get_order() and save_order() functions
# . . .
main_menu()
```

"正确！" Simon 说。"现在你知道了规则：在使用之前先定义。你刚才在 order 字典上遇到错误时看到了这个问题，现在你在这里也看到了 menu() 和 read_menu() 函数。"

"现在我们准备在 get_order() 中使用这些函数了，" 他继续说。"看看你的 menu_files.py 程序。我们第一步做了什么？"

"我们从文件读取菜单，" Erik 回答。

"很好，让我们在这里做，但放在函数内部。"

Erik 在 get_order() 函数中添加了三行：

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    return order
```

他必须调整这些行，在每一行前面加上四个空格，使它们都在同一缩进级别。

"然后再用三个 menu() 函数？" 他问 Simon。

"当然，继续吧！"

Erik 把他的函数改成了这样：

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    drink = menu(drinks, "Erik's drinks", "Choose your drink: ")
    flavor = menu(flavors, "Erik's flavors", "Choose your flavor: ")
    topping = menu(toppings, "Erik's toppings", "Choose your topping: ")
    return order
```

他为自己的工作感到自豪，看着 Simon。

"几乎正确，" Simon 说。"你复制得对，但你必须稍微改动代码来把答案存储在 order 字典中。这应该是个简单的改动，你知道怎么做。"

"啊，我明白了，" Erik 说并修改了函数。现在函数看起来像这样：

# Listing 7.2 main_menu.py

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
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

