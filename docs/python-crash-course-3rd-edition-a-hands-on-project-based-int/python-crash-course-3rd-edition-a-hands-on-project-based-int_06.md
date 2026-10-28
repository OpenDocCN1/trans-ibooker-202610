## 5 IF 语句

![](img/8f15563c16d2a98c4d3115150679b092_150_0.png)

编程通常涉及检查一组条件并根据这些条件决定采取何种行动。Python 的 `if` 语句允许你检查程序的当前状态并对该状态做出适当响应。

在本章中，你将学习编写条件测试，它允许你检查任何感兴趣的条件。你将学习编写简单的 `if` 语句，并学习如何创建更复杂的 `if` 语句序列以识别你想要的确切条件存在的情况。然后你将把这一概念应用于列表，以便你能够编写一个 `for` 循环，对列表中的大多数项目以一种方式处理，但对具有特定值的某些项目以不同的方式处理。

## 一个简单示例

以下示例展示了 if 测试如何让你正确响应特殊情况。假设你有一个汽车列表，你想打印出每辆汽车的名称。汽车名称是专有名词，因此大多数汽车的名称应以标题格式打印。然而，值 'bmw' 应以全大写形式打印。以下代码遍历汽车名称列表并查找值 'bmw'。每当值为 'bmw' 时，它会以大写形式打印，而不是标题格式：

```python
cars.py

cars = ['audi', 'bmw', 'subaru', 'toyota']

for car in cars:
    if car == 'bmw':
        print(car.upper())
    else:
        print(car.title())
```

此示例中的循环首先检查 car 的当前值是否等于 'bmw' ①。如果是，则该值以大写形式打印。如果 car 的值不是 'bmw'，则以标题格式打印：

```
Audi
BMW
Subaru
Toyota
```

此示例结合了本章中你将学习到的许多概念。让我们从查看你可以用来自检程序中条件的测试类型开始。

## 条件测试

每个 `if` 语句的核心都是一个可以评估为 `True` 或 `False` 的表达式，称为 *条件测试*。Python 使用 `True` 和 `False` 值来决定是否执行 `if` 语句中的代码。如果条件测试评估为 `True`，Python 会执行 `if` 语句后面的代码。如果测试评估为 `False`，Python 会忽略 `if` 语句后面的代码。

### 检查相等性

大多数条件测试将变量的当前值与感兴趣的特定值进行比较。最简单的条件测试检查变量的值是否等于感兴趣的值：

```python
>>> car = 'bmw'
>>> car == 'bmw'
True
```

第一行使用等号将 `car` 的值设置为 `'bmw'`，正如你已经多次看到的那样。下一行使用双等号 (==) 检查 `car` 的值是否等于 `'bmw'`。这个 *相等运算符* 如果运算符左右两侧的值匹配则返回 `True`，如果不匹配则返回 `False`。此示例中的值匹配，因此 Python 返回 `True`。

当 `car` 的值不是 `'bmw'` 时，此测试返回 `False`：

```python
>>> car = 'audi'
>>> car == 'bmw'
False
```

单个等号实际上是一个语句；你可能会将这里的第一行代码读作“将 `car` 的值设置为等于 `'audi'`。” 另一方面，双等号提出了一个问题：“`car` 的值是否等于 `'bmw'`？” 大多数编程语言都以这种方式使用等号。

### 忽略大小写检查相等性

Python 中的相等性测试是区分大小写的。例如，大写不同的两个值不被视为相等：

```python
>>> car = 'Audi'
>>> car == 'audi'
False
```

如果大小写很重要，这种行为是有利的。但如果大小写无关紧要，而你只是想测试变量的值，你可以在比较之前将变量的值转换为小写：

```python
>>> car = 'Audi'
>>> car.lower() == 'audi'
True
```

此测试无论 'Audi' 的值如何格式化都会返回 True，因为测试现在不区分大小写。`lower()` 方法不会更改最初存储在 `car` 中的值，因此你可以进行这种比较而不影响原始变量：

```python
>>> car = 'Audi'
>>> car.lower() == 'audi'
True
>>> car
'Audi'
```

我们首先将大写字符串 'Audi' 分配给变量 `car`。然后，我们将 `car` 的值转换为小写并将其与字符串 'audi' 进行比较。两个字符串匹配，因此 Python 返回 `True`。我们可以看出



`car` 中存储的值未受 `lower()` 方法的影响。

网站以类似的方式强制执行用户输入数据的某些规则。例如，某个站点可能会使用类似这样的条件测试来确保每个用户都有真正唯一的用户名，而不仅仅是另一个人用户名的大小写变化。当有人提交新用户名时，该新用户名会被转换为小写，并与所有现有用户名的 lowercase 版本进行比较。在此检查期间，像 'John' 这样的用户名将被拒绝，如果 'john' 的任何变体已被使用。

## *检查不等式*

当你想确定两个值是否不相等时，可以使用 *不等式运算符* (!=)。让我们使用另一个 `if` 语句来检查如何使用不等式运算符。我们将请求的披萨配料存储在一个变量中，然后如果对方没有点凤尾鱼，就打印一条消息：

```
*toppings.py*

requested_topping = 'mushrooms'

if requested_topping != 'anchovies':
    print("Hold the anchovies!")
```

此代码将 `requested_topping` 的值与值 `'anchovies'` 进行比较。如果这两个值不匹配，Python 返回 `True` 并执行 `if` 语句后的代码。如果两个值匹配，Python 返回 `False` 并且不运行 `if` 语句后的代码。

由于 `requested_topping` 的值不是 `'anchovies'`，因此执行了 `print()` 函数：

大多数你编写的条件表达式都会测试相等性，但有时你会发现测试不等式更有效。

## **数值比较**

测试数值非常直接。例如，以下代码检查某人是否 18 岁：

```
>>> age = 18
>>> age == 18
True
```

你也可以测试两个数字是否不相等。例如，以下代码如果给出的答案不正确就会打印一条消息：

# ***magic_number.py***

```
answer = 17
if answer != 42:
    print("That is not the correct answer. Please try again!")
```

条件测试通过，因为 `answer` 的值（17）不等于 42。由于测试通过，缩进的代码块被执行：

```
That is not the correct answer. Please try again!
```

你也可以在条件语句中包含各种数学比较，例如小于，小于或等于，大于，和大于或等于：

```
>>> age = 19
>>> age < 21
```

首先，我们定义了两个年龄，age_0 和 age_1。然后我们检查这两个年龄是否都已满 21 岁或以上 ❶。左侧的测试通过，但右侧的测试失败，因此整个条件表达式求值为 False。然后我们将 age_1 改为 22 ❷。age_1 的值现在大于 21，因此两个单独的测试都通过，导致整个条件表达式求值为 True。

为了提高可读性，你可以在单独的测试周围使用括号，但它们不是必需的。如果使用括号，你的测试将如下所示：

```
```python
(age_0 >= 21) and (age_1 >= 21)
```
```

## 使用 or 检查多个条件

关键字 or 也允许你检查多个条件，但当任一或两个单独的测试通过时就会通过。or 表达式只有在两个单独的测试都失败时才会失败。

让我们再次考虑两个年龄，但这次我们只寻找一个人超过 21 岁：

```
```python
>>> age_0 = 22
>>> age_1 = 18
❶ >>> age_0 >= 21 or age_1 >= 21
True
❷ >>> age_0 = 18
>>> age_0 >= 21 or age_1 >= 21
False
```
```

我们再次开始使用两个年龄变量。由于 age_0 的测试 ❶ 通过，因此整个表达式求值为 True。然后我们将 age_0 降低到 18。在最终测试 ❷ 中，两个测试现在都失败，整个表达式求值为 False。

## 检查值是否在列表中

有时在采取行动之前，检查列表是否包含某个值非常重要。例如，你可能希望在完成某人的网站注册之前，检查新用户名是否已存在于当前用户名列表中。在地图项目中，你可能希望检查提交的位置是否已存在于已知位置列表中。

要了解某个特定值是否已在列表中，请使用关键字 `in`。让我们考虑一下你可能为一家比萨店编写的一些代码。我们将制作一个客户为披萨请求的配料列表，然后检查某些配料是否在列表中。

```
>>> requested_toppings = ['mushrooms', 'onions', 'pineapple']
>>> 'mushrooms' in requested_toppings
True
>>> 'pepperoni' in requested_toppings
False
```

关键字 `in` 告诉 Python 检查 `'mushrooms'` 和 `'pepperoni'` 是否存在于列表 `requested_toppings` 中。这种技术非常强大，因为你可以创建一组基本值，然后轻松检查你要测试的值是否与列表中的某个值匹配。

## 检查值是否不在列表中

其他时候，知道某个值是否未出现在列表中也很重要。你可以使用关键字 `not in` 来处理这种情况。例如，考虑一个被禁止在论坛中评论的用户列表。你可以在允许某人提交评论之前检查该用户是否已被禁止：

# `banned_users.py`

```
banned_users = ['andrew', 'carolina', 'david']
user = 'marie'
```

## TRY IT YOURSELF

5-1. Conditional Tests: Write a series of conditional tests. Print a statement describing each test and your prediction for the results of each test. Your code should look something like this:

```
car = 'subaru'
print("Is car == 'subaru'? I predict True.")
print(car == 'subaru')

print("\nIs car == 'audi'? I predict False.")
print(car == 'audi')
```

Look closely at your results, and make sure you understand why each line evaluates to True or False.

Create at least 10 tests. Have at least 5 tests evaluate to True and another 5 tests evaluate to False.

5-2. More Conditional Tests: You don’t have to limit the number of tests you create to 10. If you want to try more comparisons, write more tests and add them to conditional_tests.py. Have at least one True and one False result for each of the following:

- Tests for equality and inequality with strings
- Tests using the lower() method
- Numerical tests involving equality and inequality, greater than and less than, greater than or equal to, and less than or equal to
- Tests using the and keyword and the or keyword
- Test whether an item is in a list
- Test whether an item is not in a list

## if Statements

当你理解条件测试后，你就可以开始编写 if 语句了。存在几种不同类型的 if 语句，你的选择取决于需要测试的条件数量。你在关于条件测试的讨论中看到了几个 if 语句的示例，但现在让我们更深入地探讨这个主题。

## Simple if Statements

最简单的 `if` 语句有一个测试和一个动作：

```
if conditional_test:
    do something
```

你可以在第一行放置任何条件测试，而在测试后缩进的块中放置几乎任何动作。如果条件测试求值为 True，Python 将执行 if 语句后的代码。如果测试求值为 False，Python 将忽略 if 语句后的代码。

假设我们有一个代表某人年龄的变量，我们想知道这个人是否已达到投票年龄。以下代码测试该人是否可以投票：

# voting.py

```
age = 19
if age >= 18:
    print("You are old enough to vote!")
```

Python 检查 age 的值是否大于或等于 18。是的，所以 Python 执行缩进的 `print()` 调用：

```
You are old enough to vote!
```

缩进在 `if` 语句中的作用与在 for 循环中的作用相同。如果测试通过，if 语句后所有的缩进行都将被执行，如果测试未通过，则整个缩进块将被忽略。

你可以在 `if` 语句后的块中拥有任意多行代码。让我们在该人有投票资格时再添加一行输出，询问该个人是否已注册投票：



```python
age = 19
if age >= 18:
    print("You are old enough to vote!")
    print("Have you registered to vote yet?")
```

条件测试通过，且两个 `print()` 调用都已缩进，因此两行都会被打印：

```text
You are old enough to vote!
Have you registered to vote yet?
```

如果 `age` 的值小于 18，该程序将不会产生任何输出。

## if-else 语句

通常，当条件测试通过时你会想执行一个操作，而在所有其他情况下执行不同的操作。Python 的 if-else 语法使这成为可能。if-else 块与简单的 if 语句类似，但 else 语句允许你在条件测试失败时定义要执行的操作或一组操作。

我们会在此人有投票资格时显示与之前相同的消息，但这次我们会为任何没有投票资格的人添加一条消息：

```python
age = 17
if age >= 18:
    print("You are old enough to vote!")
    print("Have you registered to vote yet?")
else:
    print("Sorry, you are too young to vote.")
    print("Please register to vote as soon as you turn 18!")
```

如果条件测试 ❶ 通过，则会执行第一块缩进的 `print()` 调用。如果测试评估为 `False`，则会执行 else 块 ❷。因为这次 age 小于 18，条件测试失败，else 块中的代码被执行：

```text
Sorry, you are too young to vote.
Please register to vote as soon as you turn 18!
```

这段代码之所以有效，是因为它只有两种可能的情况需要评估：一个人要么有投票资格，要么没有投票资格。if-else 结构在你希望 Python 总是执行两种可能操作之一的情况下效果很好。在这种简单的 if-else 链中，两种操作之一总是会被执行。

## if-elif-else 链

通常，你需要测试多于两种可能的情况，并且可以使用 Python 的 if-elif-else 语法来评估这些情况。Python 在 if-elif-else 链中只执行一个块。它按顺序运行每个条件测试，直到有一个通过。当某个测试通过时，会执行该测试后面的代码，Python 会跳过其余测试。

许多现实世界的情况涉及多于两种可能的条件。例如，考虑一个对不同年龄组收取不同费用的游乐园：

- 年龄在 4 岁以下的人免费入场。
- 年龄在 4 至 18 岁之间的人入场费为 $25。
- 年龄在 18 岁及以上的人入场费为 $40。

我们如何使用 if 语句来确定一个人的入场费率？以下代码会测试一个人的年龄组，然后打印入场价格信息：

```python
# amusement_park.py
age = 12
if age < 4:
    print("Your admission cost is $0.")
elif age < 18:
    print("Your admission cost is $25.")
else:
    print("Your admission cost is $40.")
```

`if` 测试 ① 检查一个人是否小于 4 岁。当测试通过时，会打印适当的提示，Python 会跳过其余测试。`elif` 行 ② 实际上是另一个 `if` 测试，仅在前一个测试失败时运行。此时链中，我们知道此人至少 4 岁，因为第一个测试失败了。如果此人小于 18 岁，则会打印适当的提示，Python 会跳过 `else` 块。如果 `if` 和 `elif` 测试都失败，Python 会运行 `else` 块 ③ 中的代码。

在此示例中，`if` 测试 ① 评估为 False，因此其代码块未被执行。然而，`elif` 测试评估为 True（12 小于 18），因此其代码被执行。输出是一句话，告知用户入场费用：

> Your admission cost is $25.

任何大于 17 的年龄都会导致前两个测试失败。在这些情况下，`else` 块将被执行，入场价格为 $40。

与其在 `if-elif-else` 块内打印入场价格，不如在 `if-elif-else` 链内仅设置价格，然后在链评估后进行一次单独的 `print()` 调用会更简洁：

```python
age = 12

if age < 4:
    price = 0
elif age < 18:
    price = 25
else:
    price = 40

print(f"Your admission cost is ${price}.")
```

缩进的行根据个人的年龄设置 `price` 的值，如前例所示。在 `if-elif-else` 链设置价格后，一个单独的未缩进 `print()` 调用使用该值显示报告个人入场价格的消息。

这段代码产生的输出与前一个示例相同，但 `if-elif-else` 链的目的更窄。它不是确定价格并显示消息，而是仅确定入场价格。除了更高效之外，此修订后的代码也比原始方法更容易修改。要更改输出消息的文本，你只需更改一个 `print()` 调用，而不是三个单独的 `print()` 调用。

## 使用多个 elif 块

你可以在代码中使用任意多个 `elif` 块。例如，如果游乐园要为老年人实施折扣，你可以添加另一个条件测试来确定某人是否符合老年折扣资格。假设 65 岁及以上的人支付常规入场费的一半，即 $20：

```python
age = 12

if age < 4:
    price = 0
elif age < 18:
    price = 25
elif age < 65:
    price = 40
else:
    price = 20

print(f"Your admission cost is ${price}.")
```

其中大部分代码保持不变。第二个 `elif` 块现在会检查一个人是否小于 65 岁，然后再为其分配 $40 的全额入场费。请注意，`else` 块中分配的值需要更改为 $20，因为唯一到达此块的年龄是 65 岁及以上的人。

## 省略 else 块

Python 不要求在 `if-elif` 链末尾使用 `else` 块。有时，`else` 块很有用。其他时候，使用捕获特定条件的额外 `elif` 语句会更清晰：

```python
age = 12

if age < 4:
    price = 0
elif age < 18:
    price = 25
elif age < 65:
    price = 40
elif age >= 65:
    price = 20

print(f"Your admission cost is ${price}.")
```

最后的 `elif` 块在人 65 岁或以上时分配 $20 的价格，这比一般的 `else` 块更清晰。通过此更改，每个代码块必须通过特定测试才能被执行。

`else` 块是一个兜底语句。它匹配未被特定 `if` 或 `elif` 测试匹配的任何条件，有时甚至包括无效的甚至是恶意的数据。如果你有一个特定的最终条件要测试，请考虑使用最终的 `elif` 块并省略 `else` 块。结果是，你会更有信心你的代码只会在正确条件下运行。

## 测试多个条件

`if-elif-else` 链功能强大，但仅在你只需要一个测试通过时才合适使用。一旦 Python 找到一个通过的测试，它就会跳过其余测试。这种行为是有益的，因为它高效，并且允许你测试一个特定条件。

然而，有时检查所有感兴趣的条件很重要。在这种情况下，你应该使用一系列简单的 `if` 语句，而不使用 `elif` 或 `else` 块。当可能有多个条件为 `True`，并且你希望对每个为 `True` 的条件都采取行动时，这种技术是有意义的。

让我们重新考虑比萨店的例子。如果有人要求两料比萨，你需要确保在他们的比萨上包含两种配料：

```python
# toppings.py
requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:
    print("Adding mushrooms.")
if 'pepperoni' in requested_toppings:
    print("Adding pepperoni.")
if 'extra cheese' in requested_toppings:
    print("Adding extra cheese.")

print("\nFinished making your pizza!")
```

我们从包含所请求配料的列表开始。第一个 `if` 语句检查此人是否在其比萨上要求蘑菇。如果是，则打印确认该配料的消息。对 pepperoni 的测试 ① 是另一个简单的 `if` 语句，而不是 `elif` 或 `else` 语句，因此无论之前的测试



passed or not. The last `if` statement checks whether extra cheese was requested, regardless of the results from the first two tests. These three independent tests are executed every time this program is run.

Because every condition in this example is evaluated, both mushrooms and extra cheese are added to the pizza:

```
Adding mushrooms.
Adding extra cheese.

Finished making your pizza!
```

This code would not work properly if we used an `if-elif-else` block, because the code would stop running after only one test passes. Here’s what that would look like:

```python
requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:
    print("Adding mushrooms.")
elif 'pepperoni' in requested_toppings:
    print("Adding pepperoni.")
elif 'extra cheese' in requested_toppings:
    print("Adding extra cheese.")

print("\nFinished making your pizza!")
```

The test for `'mushrooms'` is the first test to pass, so mushrooms are added to the pizza. However, the values `'extra cheese'` and `'pepperoni'` are never checked, because Python doesn’t run any tests beyond the first test that passes in an `if-elif-else` chain. The customer’s first topping will be added, but all of their other toppings will be missed:

```
Adding mushrooms.

Finished making your pizza!
```

In summary, if you want only one block of code to run, use an `if-elif-else` chain. If more than one block of code needs to run, use a series of independent `if` statements.

## Try It Yourself

- **5-3. Alien Colors #1:** Imagine an alien was just shot down in a game. Create a variable called `alien_color` and assign it a value of `'green'`, `'yellow'`, or `'red'`.

  Write an `if` statement to test whether the alien's color is green. If it is, print a message that the player just earned 5 points.

  Write one version of this program that passes the `if` test and another that fails. (The version that fails will have no output.)

- **5-4. Alien Colors #2:** Choose a color for an alien as you did in Exercise 5-3, and write an `if-else` chain.

  If the alien's color is green, print a statement that the player just earned 5 points for shooting the alien.

  If the alien's color isn't green, print a statement that the player just earned 10 points.

  Write one version of this program that runs the `if` block and another that runs the `else` block.

- **5-5. Alien Colors #3:** Turn your `if-else` chain from Exercise 5-4 into an `if-elif-else` chain.

  If the alien is green, print a message that the player earned 5 points.

  If the alien is yellow, print a message that the player earned 10 points.

  If the alien is red, print a message that the player earned 15 points.

  Write three versions of this program, making sure each message is printed for the appropriate color alien.

- **5-6. Stages of Life:** Write an `if-elif-else` chain that determines a person's stage of life. Set a value for the variable `age`, and then:

  - If the person is less than 2 years old, print a message that the person is a baby.
  - If the person is at least 2 years old but less than 4, print a message that the person is a toddler.
  - If the person is at least 4 years old but less than 13, print a message that the person is a kid.
  - If the person is at least 13 years old but less than 20, print a message that the person is a teenager.
  - If the person is at least 20 years old but less than 65, print a message that the person is an adult.
  - If the person is age 65 or older, print a message that the person is an elder.

- **5-7. Favorite Fruit:** Make a list of your favorite fruits, and then write a series of independent `if` statements that check for certain fruits in your list.

  Make a list of your three favorite fruits and call it `favorite_fruits`.

  Write five `if` statements. Each should check whether a certain kind of fruit is in your list. If the fruit is in your list, the `if` block should print a statement, such as *You really like bananas!*

## Using if Statements with Lists

You can do some interesting work when you combine lists and `if` statements. You can watch for special values that need to be treated differently than other values in the list. You can efficiently manage changing conditions, such as the availability of certain items in a restaurant throughout a shift. You can also begin to prove that your code works as you expect it to in all possible situations.

### Checking for Special Items

This chapter began with a simple example that showed how to handle a special value like `'bmw'`, which needed to be printed in a different format than other values in the list. Now that you have a basic understanding of conditional tests and `if` statements, let’s take a closer look at how you can watch for special values in a list and handle those values appropriately.

Let’s continue with the pizzeria example. The pizzeria displays a message whenever a topping is added to your pizza, as it’s being made. The code for this action can be written very efficiently by making a list of toppings the customer has requested and using a loop to announce each topping as it’s added to the pizza:

**toppings.py**

```python
requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings:
    print(f"Adding {requested_topping}.")

print("\nFinished making your pizza!")
```

The output is straightforward because this code is just a simple for loop:

```
Adding mushrooms.
Adding green peppers.
Adding extra cheese.

Finished making your pizza!
```

But what if the pizzeria runs out of green peppers? An `if` statement inside the `for` loop can handle this situation appropriately:

```python
requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings:
    if requested_topping == 'green peppers':
        print("Sorry, we are out of green peppers right now.")
    else:
        print(f"Adding {requested_topping}.")

print("\nFinished making your pizza!")
```

This time, we check each requested item before adding it to the pizza. The `if` statement checks to see if the person requested green peppers. If so, we display a message informing them why they can’t have green peppers. The `else` block ensures that all other toppings will be added to the pizza.

The output shows that each requested topping is handled appropriately.

```
Adding mushrooms.
Sorry, we are out of green peppers right now.
Adding extra cheese.

Finished making your pizza!
```

### Checking That a List Is Not Empty

We’ve made a simple assumption about every list we’ve worked with so far: we’ve assumed that each list has at least one item in it. Soon we’ll let users provide the information that’s stored in a list, so we won’t be able to assume that a list has any items in it each time a loop is run. In this situation, it’s useful to check whether a list is empty before running a `for` loop.

As an example, let’s check whether the list of requested toppings is empty before building the pizza. If the list is empty, we’ll prompt the user and make sure they want a plain pizza. If the list is not empty, we’ll build the pizza just as we did in the previous examples:

```python
requested_toppings = []

if requested_toppings:
    for requested_topping in requested_toppings:
        print(f"Adding {requested_topping}.")
    print("\nFinished making your pizza!")
else:
    print("Are you sure you want a plain pizza?")
```

This time we start out with an empty list of requested toppings. Instead of jumping right into a `for` loop, we do a quick check first. When the name of a list is used in an `if` statement, Python returns `True` if the list contains at least one item; an empty list evaluates to `False`. If `requested_toppings` passes the conditional test, we run the same `for` loop we used in the previous example. If the conditional test fails, we print a message asking the customer if they really want a plain pizza with no toppings.

The list is empty in this case, so the output asks if the user really wants a plain pizza:

```
Are you sure you want a plain pizza?
```

If the list is not empty, the output will show each requested topping being added to the pizza.

### Using Multiple Lists

People will ask for just about anything, especially when it comes to pizza toppings. What if a customer actually wants french fries on their pizza? You can use lists and `if` statements to make sure your input makes sense before you act on it.

Let’s watch out for unusual topping requests before we build a pizza. The following example defines two lists. The first is a list of available toppings at the pizzeria, and the second is the list of toppings that the user has requested. This time, each item in `requested_toppings` is checked against the list of available toppings before it’s added to the pizza:

```python
available_toppings = ['mushrooms', 'olives', 'green peppers', 'pepperoni', 'pineapple', 'extra cheese']

# ❶
requested_toppings = ['mushrooms', 'french fries', 'extra cheese']

for requested_topping in requested_toppings:
    # ❷
    if requested_topping in available_toppings:
        print(f"Adding {requested_topping}.")
    # ❸
    else:
        print(f"Sorry, we don't have {requested_topping}.")

print("\nFinished making your pizza!")
```





首先，我们定义这家披萨店可用的配料列表。注意，如果这家披萨店的配料选择是稳定的，那么这可以是一个元组。然后，我们制作一份顾客请求的配料列表。在这个例子中有一个不寻常的配料请求：`'french fries'` ❶。接下来我们遍历请求的配料列表。在循环内部，我们检查每个请求的配料是否确实在可用配料列表中 ❷。如果在，我们就把该配料添加到披萨上。如果请求的配料不在可用配料列表中，`else` 块将会运行 ❸。`else` 块会打印一条消息，告诉用户哪些配料不可用。

这段代码语法会产生干净、信息丰富的输出：

```
Adding mushrooms.
Sorry, we don't have french fries.
Adding extra cheese.

Finished making your pizza!
```

仅仅几行代码，我们就相当有效地处理了一个现实世界的情况！

## TRY IT YOURSELF

**5-8. Hello Admin:** 制作一个包含五个或更多用户名的列表，其中包括名称 `admin`。想象你正在编写代码，在用户登录网站后向每个用户打印问候语。遍历该列表，并向每个用户打印问候语。

如果用户名是 `admin`，打印特殊的问候语，例如 *Hello admin, would you like to see a status report?*

否则，打印通用问候语，例如 *Hello Jaden, thank you for logging in again.*

**5-9. No Users:** 在 *hello_admin.py* 中添加一个 `if` 测试，以确保用户列表不为空。

如果列表为空，打印消息 *We need to find some users!*

从你的列表中删除所有用户名，并确保打印出正确的消息。

**5-10. Checking Usernames:** 执行以下操作来创建一个程序，模拟网站如何确保每个人都有唯一的用户名。

创建一个名为 `current_users` 的五个或更多用户名的列表。

再创建一个名为 `new_users` 的五个用户名的列表。确保其中一两个新用户名也在 `current_users` 列表中。

遍历 `new_users` 列表，查看每个新用户名是否已被使用。如果已使用，打印一条消息，说明该人需要输入新的用户名。如果用户名未被使用，打印一条消息，说明该用户名可用。

确保你的比较不区分大小写。如果 `John` 已被使用，`JOHN` 不应被接受。 （为此，你需要制作一个包含所有现有用户的小写版本的 `current_users` 副本。）

**5-11. Ordinal Numbers:** 序数表示其在列表中的位置，例如 *1st* 或 *2nd*。大多数序数以 *th* 结尾，除了 1、2 和 3。

将数字 1 到 9 存储在列表中。

遍历该列表。

在循环内部使用 `if-elif-else` 链为每个数字打印正确的序数结尾。你的输出应读作 "1st 2nd 3rd 4th 5th 6th 7th 8th 9th"，并且每个结果应单独成行。

## Styling Your if Statements

在继续利用 `for` 循环的效率的同时，对列表中的某些项目与其他项目不同进行处理。你还重新审视了 Python 的风格建议，以确保你的日益复杂的程序仍然相对容易阅读和理解。

在第 6 章中，你将学习 Python 的字典。字典类似于列表，但它允许你关联信息。你将学习如何构建字典、遍历它们，以及将它们与列表和 `if` 语句结合使用。学习字典将使你能够模拟更广泛的现实世界情况。

OceanofPDF.com
