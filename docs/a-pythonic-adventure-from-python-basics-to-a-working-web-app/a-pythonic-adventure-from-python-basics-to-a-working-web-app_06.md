## Working with files: Being a shop manager

## In this chapter

- Erik learns what a coffee shop manager needs
- Erik reads his menus from files
- Erik writes his second Python function

"Last time you said something about a coffee shop manager," Erik started with his brother the next day. "Something about changing menus. I forgot."

"Yes, I said it might be good if your coffee shop manager could change the menus without going into Python code," Simon said.

"Yes, good idea," Erik said. "Not everyone knows Python."

"My idea is to create simple text files for each menu—call them `drinks.txt`, `flavors.txt` and `toppings.txt`. Then your program can read from those files and create lists from the items in the files."

"Why did you name them all with the `.txt` at the end?" Erik asked. "Shouldn’t they be `.docx` so the manager could edit them in Microsoft Word?"

"Good point," Simon said. "Yes, the manager might be more familiar with Word, but in our case we need just a plain text file, without fonts, or headers, or table of contents. It’s similar to your Python code—these files should have nothing but lines of plain text, and the manager should use a plain text editor to work with them. When I name them with `.txt` at the end I tell the operating system—whether it’s Windows, or macOS, or Linux-- that this file should be opened with a plain text editor and not a word processor like Word. In all those systems there is always a text editor that can edit these files. And you can also install another application for that, like we did with Mu Editor. Also, for Python it’s much easier to read from a plain text file than from a .docx file."

"Let’s create these files," Simon continued. "You can use your Mu Editor for that. Just don’t forget to add the .txt extension when saving the files. Otherwise it will automatically add .py. Create a new file, enter your drinks, each on a separate line, and save it with the name drinks.txt. Then do the same for flavors and toppings."

Erik started working. After several minutes he’s got three files.

### Listing 5.1 drinks.txt

```
coffee
chocolate
decaf
```

### Listing 5.2 flavors.txt

```
caramel
vanilla
peppermint
raspberry
plain
```

### Listing 5.3 toppings.txt

```
chocolate
cinnamon
caramel
```

YOUR TURN   Create your own menu files
Create the text files with menu items like Erik just did. Make sure they are plain text files. Try to use your favorite flavors and toppings in your menus.

"Now what?" he asked his brother.

"Now let’s learn how to work with files in Python. Let’s create a new program for that. We’ll practice a little bit and then we’ll add this code to our main program. I usually do this when I learn something new—try it in a separate simple program before adding to the main application."

Simon continued, "Create another file in the editor and save it as files.py."

Erik has done that several times already so it took him just a couple seconds.

"Working with files is a difficult topic, so let me start with a diagram," Simon said.

![](img/f6295b544c84568800930d6ec95963ae_70_0.png)

"When you work with a file in your computer you use its name. You tell your editor program to open a file called 'drinks.txt'. Your editor program then reads the file and shows you its content. Then you edit the file and save it which means you write the file on the computer disk. So far, so good?" Simon asked.

"Yes," Erik said. "But disks are used only in very old computers. In my computer it’s called SSD drive and it’s not a disk anymore. My friend Alex told me."

"You’re absolutely right!" Simon was glad to hear that from his brother. "Yes, it’s SSD now in most of the computers and yes, it’s not a disk and it is not spinning. By the way, maybe your friend Alex would like to join us? It seems that he is interested in computers. Maybe he wants to learn some programming too?"

"I’ll ask him," Erik said. "But let’s get back to my files. I see that you wrote 'computer' and 'Python' at the top of your picture. What does that mean?"

"That means," Simon started his explanation, "that in Python if you want to work with a file you have to create a special object which is usually called a file handle. You use this object to read and write the file. You use a function called open() to create such an object. You call the function open() and you pass the file name as an argument. In our case it will be open("drinks.txt"). The function returns the file handle which you put in a variable. In this case the variable is called f, but you can use any name here."

"Why is it so complicated?" Erik asked. "Why can’t we just use the file name?"

"Yes, it looks a bit complicated for the first time. The reason for that is that the file name is just a string, remember? When we want to read from the file we want to read from the file with that name, not from the string. The file name and the file itself are different things. When we use the function open() we create a connection between the file name and the file itself. We tell Python: 'please find the file named drinks.txt inside the computer and use it as a file.' Don’t worry too much about it right now. Sometimes the best way to understand something is to start using it."

"Okay," Erik said. He was still a bit confused about all that but we wanted to see how he can read his menus from the text files he just created.

"Now let’s write a simple Python program to work with files," Simon said. "Now go to the tab in your editor where you have your files.py program opened.

Look at my diagram: you have to call the function open(), pass the file name such as 'drinks.txt', and store the result in the variable f. Can you write it?"

![](img/f6295b544c84568800930d6ec95963ae_71_0.png)

"Let me try," Erik said and wrote this.

```
f = open("drinks.txt")
```

"Good!" Simon said. "Now you have a file object named f and you can read from it. To do that you call a method called read(). Methods look and behave very similar to functions, but methods are applied to objects. We’ll talk about objects later but for now all you have to know is that to call a method of an object you use the object’s name, then you put a dot and then the method name with parentheses, similar to calling a function. Like this: f.read(). Methods also can return something, similar to functions. So you have to store the result somewhere. What do you think this method read() will return when you call it?"

"What’s inside the file, I think," Erik answered.

"Absolutely correct!" Simon said. "Save it in a variable called drinks and then try to print it."

Erik wrote the following.





```
f = open("drinks.txt")
drinks = f.read()
print(drinks)
```

"现在试着运行它吧，" Simon 建议道。

Erik 点击了 Run 并得到了输出。

```
coffee
chocolate
decaf

>>>
```

YOUR TURN   Read from file

编写同样的简短程序并尝试从 'drinks.txt' 文件中读取。确保你能打印出整个文件的内容。

“它工作了！”他非常高兴。他的 Python 程序打开了一个文件，读取了它，并在页面上打印了出来！“现在我知道如何从 Python 打印我的菜单了！让我为另外两个文件也写同样的程序吧！”

“对，” Simon 说。“但这并不完全是我们想要的。”

“为什么？” Erik 不能理解。

“记住，在你的程序中你不仅要打印出你的菜单，还要让用户从菜单中选择，然后在列表中找到该项目，对吧？”

“是的，但这不是一个列表吗？看起来像一个列表，” Erik 问。

“它看起来像一个列表，但它不是列表。它是一个字符串，” Simon 说。“当你调用 read() 方法时，你把整个文件的内容复制到了名为 drinks 的变量中。所以这个变量只是一个很大的字符串。如果你不相信我，你可以在这里测试一下。你看输出窗口里的那三个大于号吗？你可以在这里输入任何 Python 命令来继续使用你的程序，就像我们用 REPL 做的一样，记得吗？在这里输入这个：type(drinks) 你就会看到这个变量的类型。”

Erik 这样做了并看到了这个：

```
>>> type(drinks)
<class 'str'>
>>>
```

“你看，Python 说它是一个字符串，” Simon 说。“而我们需要的是一个列表。”

“我们该怎么办？” Erik 问。

“幸运的是，Python 开发者知道我们可能需要这个，并为文件对象创建了另一个方法。它叫做 readlines()。试着把你的 read() 改成 readlines() 看看会发生什么。在再次运行程序之前别忘了点击 Stop。”

Erik 把他的程序改成了如下内容。

```
f = open("drinks.txt")
drinks = f.readlines()
print(drinks)
```

他点击了 Stop 然后再次点击 Run 并得到了这个输出。

```
['coffee\n', 'chocolate\n', 'decaf\n']
>>>
```

“再试着检查一下它的类型，” Simon 建议。

Erik 切换到输出窗口并输入：

```
>>> type(drinks)
<class 'list'>
>>>
```

YOUR TURN   Check Python types
重复 Erik 刚刚做的检查。你能看出字符串和列表之间的区别吗？

“它是一个列表！”他说。“但那些斜杠-n 字符是什么？我不在 drinks 文件里看到它们。”

“你在文件里看不到它们，但它们在那里。这些是不可见字符。当你看到这个反斜杠-n 时，它是一个叫做 '换行' 字符的单个字符。它告诉计算机应该在下一行的开头打印下一个项目。没有它，你的所有饮料都会像这样打印：coffeechocolatedecaf。你不想要那样，对吧？” Simon 微笑着。

“当然不！” Erik 说。“但我们在列表中不需要它们，对吧？我认为菜单列表应该看起来像我在主程序中的那样，对吧？”

“你完全正确。而且同样，Python 开发者为此创建了一个非常有用的方法。它叫做 strip()，它会从字符串的两端移除不可见字符。我们只需要把它应用到列表中的每个项目。你觉得我们这里应该用什么？”

“一个循环？” Erik 建议。

“对，一个循环！” Simon 说。“我们将遍历列表，并用 strip() 方法移除那些换行字符。”

Simon 停顿了一下，思考着。然后他继续说，“有几种方法可以做到这一点。有些更短，但更难理解。让我们使用更容易阅读和跟随的那一种。实际上编程中有一个好规则：在选择做某事的不同方法时，始终使用更容易阅读和跟随的那一种。如果有人在阅读你的代码，他们会感谢你的。即使是你自己——如果你在三个月后阅读自己的代码。”

他迅速画了一张图表。

![](img/f6295b544c84568800930d6ec95963ae_74_0.png)

“让我们使用一个临时列表从文件中读取。然后我们将遍历那个临时列表，转换每个项目，并把它追加到新列表中。而那个新列表我们就叫做 drinks。然后我们对 flavors 和 toppings 重复同样的操作。我们可以为它们全部使用同一个临时变量。让我来帮你吧，” Simon 开始在 Erik 的程序中输入。这里是他完成后它看起来像什么。

```
f = open("drinks.txt")
temp = f.readlines()
drinks = []
for item in temp:
    new_item = item.strip()
    drinks.append(new_item)

print(drinks)
```

他点击了 Run，他们看到了结果：

```
['coffee', 'chocolate', 'decaf']
>>>
```

YOUR TURN   Remove the newline characters
使用 `strip()` 方法移除菜单项中的换行字符。

“看起来更好，不是吗？”他问 Erik。“现在继续为其他菜单文件做同样的事情吧。”

Erik 开始在他的程序上工作，当他几乎完成 `flavors.txt` 文件时，他惊呼道，“等等！我在重复自己！你告诉我我不应该重复自己。”

Simon 微笑着。他很高兴他的小弟弟这么快就掌握了这个概念。“我们怎样才能*不*重复自己呢？”他问道。

“写一个函数？” Erik 说。

“是的，完全正确！看看这段代码：什么是一样的，什么在变化？什么将成为参数，什么将是这个函数返回的？”

Erik 开始大声思考，“我打开不同的文件，所以文件名应该是参数，对吧？”

“正确！” Simon 确认。“你想返回什么？”

“我认为我返回包含菜单选项的列表。 当然，在我们移除了那些换行字符之后。但我应该怎么称呼这个列表呢？”

“你可以随意称呼它，因为它在外部是*不可见*的。比如称它为 `result`。然后你可以在函数末尾写 `return result`。当你调用该函数时，那个 `result` 列表会被*赋值*给主程序中的变量，比如 `drinks` 或 `flavors`，取决于你读取的文件。*函数内部*变量始终称为 `result`，但*函数外部*，在主程序中，你可以把它赋给任何变量。”

![](img/f6295b544c84568800930d6ec95963ae_76_0.png)

“继续写那个函数吧！” Simon 鼓励他的弟弟。“还记得怎么做吗？从 def 开始；给函数命名，例如 read_menu；将 filename 作为参数传递；并复制我们刚刚写的那段代码。”

“好的，我试试，” Erik 说并开始编写他的第二个 Python 函数。

Simon 帮了他的弟弟一点点，这里是他们一起写的内容。

## Listing 5.4 files.py

```
def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)

    return result

drinks = read_menu("drinks.txt")
print(drinks)
flavors = read_menu("flavors.txt")
print(flavors)
toppings = read_menu("toppings.txt")
print(toppings)
```

Erik 保存了文件并点击了 Run。当然，他得到了预期的结果。

```
['coffee', 'chocolate', 'decaf']
['caramel', 'vanilla', 'peppermint', 'raspberry', 'plain']
['chocolate', 'cinnamon', 'caramel']
>>>
```

## YOUR TURN Create the read_menu function

以 Erik 的方式创建 read_menu 函数。确保文件名中没有拼写错误。如果有会怎样？试着更改文件名，看看 Python 会给出什么错误。别忘了修复文件名，让你的程序再次工作。

“这是我的第二个函数而且它工作了！”他自豪地对他的哥哥说。

“是的，你已经开始构建自己的函数库了，很棒！” Simon 说。“现在让我们把你的新函数复制到主程序中。不要复制 print() 行——我们只是为了测试才使用它们。我想那个文件叫做 menu_function.py，对吧？”

“对，” Erik 说。“但我应该把我的函数放在那个文件的开头还是结尾？”

“Python 的规则是，你应该在开始使用函数之前定义它。因为这个原因，通常所有函数都放在文件的开头，在主程序之前。你可以把它放在你的第一个函数之后。”

“好的，” Erik 说并开始工作。这里是他得到的结果。

## Listing 5.5 menu_files.py

```
def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    ...
    # this function didn't change
    ...
    return answer

def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)

    return result

drinks = read_menu("drinks.txt")
flavors = read_menu("flavors.txt")
toppings = read_menu("toppings.txt")

drink = menu(drinks)
flavor = menu(flavors, "Erik's flavors", "Choose your flavor: ")
topping = menu(toppings, "Erik's toppings", "Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drink)
print("Flavor: ", flavor)
print("Topping: ", topping)
print("Thanks for your order")
```

**YOUR TURN**   **Copy your function to your main program**
**把新的 read_menu() 函数复制到你的主程序中并试试。**

他测试了程序，它正如之前一样工作！

“这不错，” Simon 说。“现在试着在 toppings 文件中添加一些内容，例如。然后看看



if it changes the menu."

Erik opened the `toppings.txt` file and added 'vanilla powder' to the end of file and saved it. He ran the program again and indeed there was the additional line at the last menu: 4 vanilla powder!

## YOUR TURN Add another item

Add a new item to one of the menus. Change one of the items. Don’t forget to save the menu files after you changed them. Check whether your program prints the updated menus.

"This is good, I like it!" he said. "Now anybody who can edit a text file can change the menu! Wait..." he had an idea. "So I can put anything in these menus! Ice cream or sandwiches or... Cool, I like it! I should tell my friend Alex about it—he likes LEGO minifigures. Maybe he can use this program to exchange figures with friends!"

"Exactly right!" Simon said. "I’m glad you have so many ideas on how to use your program, this is great! I have some ideas too but we’d better talk about them tomorrow. Also tomorrow we have to create the main menu."

"What do you mean?"

"You see, currently you have to start your program every time you want to take an order. You take the order, you print it, and your program finishes. It would be better if your program could return to the initial dialogue where you ask the customer’s name."

"Yes, right," Erik agreed. "It should be like a kiosk where you order something, press 'Done', and it goes to the first screen with 'Welcome to our shop'. Yes, let’s do it!"

"Let’s recap today’s progress," Simon suggested. "What did we do today?"

"First, you said that the coffee shop manager will want to edit our menus in files. Then I wrote three files with menus for main drinks, flavors, and toppings."

"Very good, what’s next?"

"Then I opened the files and read from them. I could read line by line but then I had those strange 'backslash-n' characters. And then we used the `strip()` method to remove them."

"Good," Simon said. "And did you remember what I told you about objects?"

"Not really. You said that the file is an object in Python and it’s not the same as its name. And also you said that functions with objects are called 'methods'."

"Yes, everything is right," Simon said. "Objects is a difficult topic, we will talk about it more later. For now we just use them and their methods, but we will learn more about them later. And we’ll create our own objects and methods too—like we did with functions."

"Right!" Erik said. "You reminded me—I wrote my second Python function! And it worked!"

"Indeed! You are becoming a serious programmer now!" Simon said and smiled. "Let’s take a rest for now. Tomorrow we’ll make your program even better!"

## New things you have learned today

- **What it means to open a file**
  You have learned the difference between the file name and the file handle inside the program.
- **What is \n and how to remove it from strings**
  You have learned that the \n symbol means "start a new line". We don’t need it in our menu items so we used the `strip()` function to remove it.
- **What you return from a function is assigned to a variable in the main program**
  You have learned that the variable inside a function is not *visible* in the main program. To pass its value we have to *return* that variable from the function and *assign* its value to another variable in the main program.

## Code for this chapter

You can find the code for this chapter here:
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch05
