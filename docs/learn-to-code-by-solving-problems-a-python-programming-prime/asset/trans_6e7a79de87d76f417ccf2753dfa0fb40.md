
我们必须谨慎选择牧场 1 的草类型。我们不想让某头奶牛的两个牧场拥有相同的草类型，因为那会违反规则！我们还没有选择任何草类型，所以无论为牧场 1 选择什么都不会出错。不过，由于我们希望草类型的编号尽可能小，我们会选择草类型 1。

我会把草类型的决定记录在表格中。以下是我们刚刚做出的决定：牧场 1 使用草类型 1：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |

接着往下看。我们应该为牧场 2 选择什么草类型？关心牧场 2 的奶牛是奶牛 1、4 和 5，所以我们关注这些奶牛。奶牛 4 的其中一个牧场是牧场 1，而我们已经为牧场 1 选择了草类型 1，所以草类型 1 被排除，不能用于牧场 2。如果我们在牧场 2 使用草类型 1，那么奶牛 4 的两个牧场就拥有了相同的草类型，这会违反规则。然而，奶牛 1 和 5 并没有排除任何其他草类型，因为我们还没有为它们的牧场选择草类型。因此，我们选择草类型 2，即编号最小的可用草类型。目前的情况如下：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |
| 2 | 2 |

我们应该为牧场 3 选择什么草类型？唯一关心牧场 3 的奶牛是奶牛 2。奶牛 2 的牧场是牧场 3 和 5。但该奶牛并没有排除任何草类型，因为我们还没有为牧场 5 分配草类型！为了得到最小编号，我们会在牧场 3 使用草类型 1。下一个快照如下：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |
| 2 | 2 |

| 牧场 | 草类型 |
|---|---|
| 3 | 1 |

在这里，我可以看到自顶向下设计中的三个任务逐渐成形。第一，我们需要获取关心当前牧场的奶牛。第二，我们需要确定这些奶牛会排除哪些草类型。第三，我们需要选择未被排除的最小编号的草类型。其中每一个都是函数的绝佳候选。

我们继续。有三头奶牛关心牧场 4：奶牛 0、1 和 3。奶牛 0 没有排除任何草类型，因为我们还没有为它的牧场分配草类型。奶牛 1 排除了草类型 2，因为我们为牧场 2（它的另一个牧场）分配了草类型 2。奶牛 3 排除了草类型 1，因为我们为牧场 1（它的另一个牧场）分配了草类型 1。那么，编号最小的可用草类型是 3，所以牧场 4 使用草类型 3：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |

接着看牧场 5。关心牧场 5 的奶牛是奶牛 0、2 和 5。奶牛 0 排除了草类型 3；奶牛 2 排除了草类型 1；奶牛 5 排除了草类型 2。因此草类型 1、2 和 3 都被排除了。我们唯一的选择是草类型 4。

好险！我们差点就用光了草类型。幸运的是，没有其他奶牛关心牧场 5 并排除草类型 4。

或者，等等。这根本不是运气，因为题目描述中有这样一句话：“保证每个牧场最多成为三头奶牛的最爱。”这意味着每个牧场最多只能被排除三种草类型。我们永远不会走投无路！而且我们甚至不必担心过去的选择会对下一个决定产生什么影响。无论过去做了什么，我们总是至少有一种可用的草类型。

我们把牧场 5 加入表格：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |

还剩三个牧场。但是没有奶牛关心它们，所以我们只需在每种情况下使用草类型 1。结果如下：

| 牧场 | 草类型 |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |
| 6 | 1 |
| 7 | 1 |
| 8 | 1 |

我们可以从上到下读取草类型，得到这个示例的正确输出。输出如下：

12134111

## 自顶向下设计

在清楚理解了我们需要完成的任务之后，我们转而对这个问题进行自顶向下设计。

## 顶层

在上一节中，我们通过一个测试用例发现了三个任务。在程序解决其中任何一个任务之前，我们需要读取输入，所以这是第四个任务。我们还需要写出输出。这需要一些思考，以及几行代码，所以我们就把它称为第五个任务。

以下是我们的五个主要任务：

- 1. 读取输入。
- 2. 找出关心当前牧场的奶牛。
- 3. 为当前牧场排除草类型。
- 4. 为当前牧场选择编号最小的草类型。
- 5. 写出输出。

和我们在第 6 章解决 Action Figures 问题时一样，我们会从包含 TODO 注释的框架开始，并在解决每个任务时移除相应的 TODO。

我们首先以注释为主。由于我们需要在开始时打开文件，并在结束时关闭文件，我也添加了这些代码。

我们从这里开始：

```
# Main Program

input_file = open('revegetate.in', 'r')
output_file = open('revegetate.out', 'w')

# TODO: Read input

# TODO: Identify cows that care about pasture

# TODO: Eliminate grass types for pasture

# TODO: Choose smallest-numbered grass type for pasture

# TODO: Write output

input_file.close()
output_file.close()
```

## 任务 1：读取输入

读取输入的第一行，其中包含整数 n 和 m，是我们已经知道如何做的事情。它足够直接，我认为不需要为它编写函数，所以我们就直接做。接下来，我们需要读取 m 头奶牛的牧场信息，这里似乎值得使用一个函数。让我们移除“读取输入”注释中的 TODO，处理输入的第一行，并调用 read_cows 函数，我们稍后会编写该函数：

```
# Main Program

input_file = open('revegetate.in', 'r')
output_file = open('revegetate.out', 'w')

# Read input
lst = input_file.readline().split()
num_pastures = int(lst[0])
num_cows = int(lst[1])
① favorites = read_cows(input_file, num_cows)

# TODO: Identify cows that care about pasture

# TODO: Eliminate grass types for pasture

# TODO: Choose smallest-numbered grass type for pasture

# TODO: Write output

input_file.close()
output_file.close()
```

我们正在调用的 read_cows 函数 ① 会接收一个已打开供读取的文件，并读取每头奶牛的两个最爱牧场。它会返回一个列表的列表，其中每个内部列表包含给定奶牛的两个牧场编号。代码如下：

```
def read_cows(input_file, num_cows):
    """
    input_file is a file open for reading; cow information is next to read.
    num_cows is the number of cows in the file.

    Read the cows' favorite pastures from input_file.
    Return a list of each cow's two favorite pastures;
    each value in the list is a list of two values giving the
    favorite pastures for one cow.
    """
    favorites = []
```

```
for i in range(num_cows):
    ① lst = input_file.readline().split()
    lst[0] = int(lst[0])
    lst[1] = int(lst[1])
    ② favorites.append(lst)
return favorites
```

这个函数将奶牛们的最爱牧场累积到 favorites 列表中。它使用一个 for 循环，循环 num_cows 次，每次处理一头奶牛。我们需要这个循环，因为要读取的行数取决于文件中的奶牛数量。

在循环的每次迭代中，我们读取下一行，并将其分成两个组成部分 ①。然后使用 int 将这些组成部分从字符串转换为整数。当我们把这个列表追加到 favorites ② 时，我们追加的其实是一个包含两个整数的列表。

最后，我们返回这个最爱牧场的列表。

在继续之前，让我们确保知道如何调用这个函数。我们会单独调用它来练习，而不是在我们正在构建的更大程序中调用。像这样测试函数很有用，这样我们可以修复在过程中可能发现的任何错误。

使用文本编辑器创建一个名为 revegetate.in 的文件，内容如下（与之前研究的测试用例相同）：

```
8 6
5 4
2 4
3 5
4 1
2 1
5 2
```

现在，在 Python shell 中输入我们的 read_cows 函数代码。我们这样调用 read_cows：

```
>>> input_file = open('revegetate.in', 'r')
① >>> input_file.readline()
'8 6\n'
② >>> read_cows(input_file, 6)
[[5, 4], [2, 4], [3, 5], [4, 1], [2, 1], [5, 2]]
```

read_cows 函数只读取奶牛的信息。由于我们是在程序之外单独测试这个函数，我们需要

