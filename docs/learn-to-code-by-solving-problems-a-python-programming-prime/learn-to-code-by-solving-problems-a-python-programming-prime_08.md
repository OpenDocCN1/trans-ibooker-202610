# DESIGNING ALGORITHMS WITH COMPLETE SEARCH

![](img/3e15323aee4cec3bf673834bfafe8024_296_0.png)

An *algorithm* is a sequence of steps that solves a problem. For each problem in this book, we solved it by writing an algorithm in the form of Python code. We’ll focus in this chapter on designing algorithms. When faced with a new problem, sometimes it’s hard to know what to do to solve it. What algorithm should we write? Fortunately, we don’t need to start from scratch each time. Computer scientists and programmers have identified several general types of algorithms, and it’s likely that at least one of them can be used to solve our problem.

One type of algorithm is called a *complete search* algorithm; it involves trying all candidate solutions and choosing the best one. For example, if the problem asks us to find a maximum, we try all solutions and choose the largest; if the problem asks us to find a minimum, we try all solutions and choose the smallest. Complete-search algorithms are also known as *brute-force* algorithms, but I’ll avoid that term. It’s true that the computer is powering its way through, checking solution after solution, but there’s nothing brute force about what we’re doing as algorithm designers.

We used a complete-search algorithm to solve Village Neighborhood in Chapter 5. We were asked to find the smallest size of the neighborhoods, and we did that by looking at each neighborhood and remembering the size of the smallest one. In this chapter, we’ll use complete-search algorithms to solve other problems. We’ll see that it can take considerable ingenuity to determine what exactly to search.

We’ll solve two problems using complete search: determining which lifeguard to fire and identifying the minimum cost to meet ski training camp requirements. Then we’ll see a third problem, counting triples of cows that meet given observations, that requires we go a little further.

## Problem #21: Lifeguards

In this problem, we’ll need to determine which lifeguard to fire that leaves us with the maximum schedule coverage of a pool. We’ll try separately firing each one and observing the results—that’s a complete-search algorithm!

This is USACO 2018 January Bronze Contest problem Lifeguards.

### The Challenge

Farmer John has purchased a swimming pool for his cows. The pool is open from time 0 to time 1000.

Farmer John hires n lifeguards to monitor the pool. Each lifeguard monitors the pool for a given interval of time. For example, a lifeguard might start at time 2 and end at time 7. I’ll denote such an interval as 2–7. The number of units of time covered by an interval is the ending time minus the starting time. For example, the lifeguard whose time interval is 2–7 covers 7 − 2 = 5 units of time. Those time units are from time 2 to 3, 3 to 4, 4 to 5, 5 to 6, and 6 to 7.

Unfortunately, Farmer John only has enough money to pay for n − 1 lifeguards, not n lifeguards, so he must fire one lifeguard.

Determine the maximum number of units of time that can still be covered after firing one lifeguard.

### Input

Read input from the file named lifeguards.in.

The input consists of the following lines:

- A line containing n, the number of lifeguards who were hired. n is between 1 and 100.
- n lines, one per lifeguard. Each line gives the time when the lifeguard starts, a space, and the time when the lifeguard ends. The start and end times are all integers between 0 and 1,000 and are all distinct.

### Output

Write output to the file named lifeguards.out.

Output the maximum number of units of time that can be covered by n − 1 of the lifeguards.

The time limit for solving each test case is four seconds.

### Exploring a Test Case

Let’s explore a test case to help justify why a complete-search algorithm makes sense for this problem. Here’s the test case:

```
4
5 8
10 15
17 25
9 20
```

One simple rule you might try to use to solve this problem is to fire the lifeguard with the shortest time interval. That makes some intuitive sense, because it seems as though that lifeguard contributes the least to covering the pool.

Does this rule give us a correct algorithm? Let’s see. It tells us to fire the 5–8 lifeguard, since that lifeguard has the shortest time interval. That leaves us with the three lifeguards whose time intervals are 10–15, 17–25, and 9–20. These three remaining lifeguards cover exactly the interval 9–25, which consists of 25 − 9 = 16 units of time. Is 16 the correct answer?

Unfortunately, no. It turns out that what we should have done is fire the 10–15 lifeguard. If we do that, then we’re left with the three lifeguards whose time intervals are 5–8, 17–25, and 9–20. These three remaining lifeguards cover the intervals 5–8 and 9–25. (Careful: they don’t cover the unit of time from 8 to 9.) The first of these intervals covers 8 − 5 = 3 units of time, and the second covers 25 − 9 = 16 units of time, for a total of 19 units of time.

The correct answer is 19, not 16. Firing the lifeguard with the shortest time interval didn’t work.

It’s not easy to come up with a simple rule that always works to solve this problem. We don’t need to worry, though: with a complete-search algorithm, we dodge this requirement entirely.

Here’s what our complete-search algorithm will do to solve our test case:

- 1. First, it will ignore the first lifeguard and determine the number of units of time that the three remaining lifeguards cover. It will obtain an answer of 16. It will remember 16 as the score to beat.
- 2. Next, it will ignore the second lifeguard and determine the number of units of time that the three remaining lifeguards cover. It will obtain an answer of 19. Since 19 is greater than 16, it will remember 19 as the score to beat.
- 3. Next, it will ignore the third lifeguard and determine the number of units of time that the three remaining lifeguards cover. It will obtain an answer of 14. The score to beat is still 19.
- 4. Finally, it will ignore the fourth lifeguard and determine the number of units of time that the three remaining lifeguards cover. It will obtain an answer of 16. The score to beat is still 19.

Having considered the ramifications of firing each lifeguard, the algorithm concludes that 19 is the correct answer. There can be no better answer than this, because we tried every option! We performed a complete search of the possible solutions.

### Solving the Problem

To use complete search, it’s often helpful to begin by writing a function that solves the problem for a particular candidate solution. We can then call that function many times, once for each candidate solution.

### Firing One Lifeguard

Let’s write a function to determine the number of time units that are covered when one particular lifeguard is fired. Listing 9-1 shows the code.

```
def num_covered(intervals, fired):
    """
    intervals is a list of lifeguard intervals;
```




每个区间都是一个 [start, end] 列表。
fired 是要解雇的救生员的索引。

返回除被解雇者外所有救生员覆盖的时间单位数。

```
covered = set()
for i in range(len(intervals)):
    if i != fired:
        interval = intervals[i]
        for j in range(interval[0], interval[1]):
            covered.add(j)
return len(covered)
```

*清单 9-1. 当一名特定的救生员被解雇时的求解*

第一个参数是救生员时间区间的列表；第二个参数是要解雇的救生员的索引。将代码输入到 Python shell 中。下面是该函数的两个示例调用：

```
>>> num_covered([[5, 8], [10, 15], [9, 20], [17, 25]], 0)
16
>>> num_covered([[5, 8], [10, 15], [9, 20], [17, 25]], 1)
19
```

这些调用证实：如果我们解雇救生员 0，可以覆盖 16 个时间单位；如果我们解雇救生员 1，可以覆盖 19 个时间单位。

现在让我们理解这个函数是如何运行的。我们首先创建一个集合，用于存放被覆盖的时间单位。每当一个时间单位被覆盖时，代码就会将该时间单位的起点加入集合。例如，如果从 0 到 1 的时间单位被覆盖，代码就会把 0 加入集合；如果从 4 到 5 的时间单位被覆盖，它就会把 4 加入集合。

我们遍历救生员的时间区间。如果某个救生员没有被解雇，我们就遍历该救生员的时间区间，逐一考虑每个被覆盖的时间单位。如前面所说，我们将这些时间单位逐个加入集合。回顾一下，集合不会保留重复值；因此即使我们多次尝试加入同一个时间单位，也不用担心。我们已经遍历了所有未被解雇的救生员，并将所有被覆盖的时间单位加入集合。因此，我们只需返回集合中值的个数即可。

## 主程序

我们程序的主要部分在清单 9-2 中。它使用 num_covered 函数来确定分别解雇每个救生员时覆盖的时间单位数。请务必在输入此代码之前先输入我们的 num_covered 函数（清单 9-1），这样才能得到完整的问题解决方案。

```
input_file = open('lifeguards.in', 'r')
output_file = open('lifeguards.out', 'w')

n = int(input_file.readline())

intervals = []

for i in range(n):
    ① interval = input_file.readline().split()
    interval[0] = int(interval[0])
    interval[1] = int(interval[1])
    intervals.append(interval)

max_covered = 0

② for fired in range(n):
    ③ result = num_covered(intervals, fired)
    if result > max_covered:
        max_covered = result

output_file.write(str(max_covered) + '\n')

input_file.close()
output_file.close()
```

清单 9-2：主程序

这里我们使用的是文件，而不是标准输入和标准输出。

程序首先读取救生员的数量，然后使用 range for 循环读取每个救生员的时间区间。我们从输入①中读取每个时间区间，将其每个组成部分转换为整数，然后以包含两个值的列表形式追加到我们的 intervals 列表。

我们使用 max_covered 变量来跟踪可覆盖的最大时间单位数。

现在，我们使用 range for 循环②分别解雇每个救生员。我们调用 num_covered③来确定在解雇某个救生员的情况下覆盖的时间单位数。每当能够覆盖更多的时间单位时，我们就更新 max_covered。

当该循环结束时，我们已经检查了解雇每个救生员所能覆盖的时间单位数，并记住了最大值。我们输出这个最大值，从而解决问题。

请放心将我们的代码提交给 USACO 评测系统。对于 Python 代码，该系统每个测试用例的时间限制为四秒，但我们的解决方案应该远达不到这个限制。例如，我刚刚在这里运行了代码，每个测试用例都在 130 毫秒内完成。

## 我们程序的效率

我们的代码之所以如此快，是因为救生员数量非常少——最多只有 100 个。如果救生员数量很多，那么我们的代码将无法在时间限制内解决问题。如果有几百个救生员，我们还能应付。如果救生员多达 3,000 或 4,000 个，我们也许勉强能通过。不过，如果再多，我们的代码就会太慢了。例如，如果有 5,000 个救生员，我们很可能无法按时完成。我们需要设计一个新算法，很可能是比完全搜索更快的方法。

你可能会认为 5,000 个救生员是一个巨大的数字，因此我们的算法无法处理这么多也没关系。但事实并非如此！回想一下第 8 章中的“电子邮件地址”问题。在那里，我们需要处理多达 100,000 个电子邮件地址。再回想一下同一章中的“城市与州”问题。在那里，我们需要处理多达 200,000 个城市。比较起来，5,000 个救生员并不算多。

完全搜索解决方案通常适用于少量输入。而大规模测试用例往往是完全搜索解决方案失效的地方。

我们针对救生员问题的完全搜索解决方案无法很好地处理大规模测试用例，原因在于它做了大量重复工作。设想我们正在解决一个有 5,000 个救生员的测试用例。我们会解雇救生员 0，并调用 `num_covered` 来确定剩余救生员覆盖的时间单位数。然后，我们会解雇救生员 1，并再次调用 `num_covered`。这次 `num_covered` 所做的事情与上一次调用类似。毕竟，情况并没有太大变化。唯一的变化是救生员 0 回来了，而救生员 1 被解雇。其他 4,998 名救生员和之前一样！但是 `num_covered` 并不知道这一点。它会再次对所有救生员进行完整遍历。当我们解雇救生员 2、然后救生员 3 等时，同样的事情不断发生。每次，`num_covered` 都会从头开始做所有工作，而不会借鉴任何之前的工作成果。

请记住，完全搜索算法虽然有用，但也有其局限性。对于我们要解决的新问题，完全搜索算法是一个有用的起点，即使它最终被证明过于低效。因为设计该算法的过程可能会加深我们对问题的理解，并带来解决它的新思路。

在下一节中，我们将看到另一个可以使用完全搜索的问题。

## 概念检查

下面的 num_covered 版本正确吗？

```
def num_covered(intervals, fired):
    """
    intervals is a list of lifeguard intervals;
    each interval is a [start, end] list.
    fired is the index of the lifeguard to fire.

    Return the number of time units covered by all lifeguards
    except the one fired.
    """
    covered = set()
    intervals.pop(fired)
    for interval in intervals:
        for j in range(interval[0], interval[1]):
            covered.add(j)
    return len(covered)
```

- A. 是
- B. 否

答案：B。这个函数会将被解雇的救生员从救生员列表中移除。这是不允许的，因为文档字符串中没有说明该函数会修改列表。使用这个版本的函数，我们的程序将会在许多测试用例上失败，因为救生员信息会随着时间推移而丢失。例如，当我们测试解雇救生员 0 时，救生员 0 会从列表中被移除。当我们之后测试解雇救生员 1 时，遗憾的是救生员 0 仍然不在列表中！如果你希望使用一个将被解雇的救生员从列表中移除的函数版本，你需要使用列表的副本，而不是原始列表。

## 问题 #22：滑雪山丘

有时候，问题描述会清楚地说明在完全搜索解决方案中我们应该搜索什么。例如，在“救生员”问题中，我们需要解雇一名救生员，因此尝试解雇每一名救生员是有意义的。其他时候，我们需要更有创造力来确定要搜索的内容。在阅读下一个问题时，想一想在完全搜索解决方案中你会搜索什么。

这是 USACO 2014 年 1 月铜组竞赛题“滑雪场设计”（Ski Course Design）。

## 挑战

农夫约翰的农场里有 n 座山丘，每座山丘的高度在 0 到 100 之间。他想将自己的农场注册为滑雪训练营。

只有当最高山丘与最矮山丘之间的高度差不超过 17 时，农场才能注册为滑雪训练营。因此，农夫约翰可能需要增加一些山丘的高度，同时降低另一些山丘的高度。他只能以整数数量来改变高度。

将一座山丘的高度改变 x 个单位的成本是 x²。例如，将一座山丘从高度 1 改为高度 4 需要花费 (4 − 1)² = 9。

确定农夫约翰为了使农场能够注册为滑雪训练营而改变各山丘高度所需支付的最低金额。

## 输入

从名为 skidesign.in 的文件中读取输入。

输入由以下行组成：


这个总和是 (40 − 23)² + (40 − 16)² + (40 − 2)² = 2,309。它大于 490（我们当前的最小成本），因此 490 仍然是要超越的成本。（请记住，在这个问题中我们是在最小化农夫约翰的成本，而在 Lifeguards 问题中我们是在最大化覆盖范围。）

第三座山丘的高度是 16，这给我们范围 16–33。该范围外有两座山丘，高度分别为 40 和 2。因此该范围的总成本为 (40 − 33)² + (16 − 2)² = 245。新的要超越的成本是 245！

第四座山丘的高度是 2，这给我们范围 2–19。如果你计算这个范围的成本，应该得到 457。

我们使用上述算法得到的最小成本是 245。245 是答案吗？我们完成了吗？

不，都不是！事实证明最小成本是 221。有两个范围能给出这个最小成本：12–29 和 13–30。没有哪座山丘的高度是 12。同样，也没有哪座山丘的高度是 13。因此我们不能把山丘高度用作范围的可能下限。

思考一下，一个正确的完全搜索算法可能是什么样子，一个保证不会漏掉任何范围的算法。

这里有一个保证能得到正确答案的方案。我们首先计算范围 0–17 的成本。然后计算范围 1–18 的成本。接着是 2–19。再是 3–20。然后是 4–21，依此类推。我们逐个测试每一个可能的范围，并记住得到的最小成本。我们测试的范围与山丘的高度无关。既然我们测试了每一个可能的范围，我们不可能漏掉最优的那个。

我们应该测试哪些范围？应该走多高？我们应该测试范围 50–67 吗？是的。那么范围 71–88 呢？也是的。那么 115–132 呢？不！不是那一个。

我们将检查的最后一个范围是 100–117。原因与问题描述中的保证有关，即任何山丘的高度至多为 100。

假设我们计算范围 101–118 的成本。即使不知道山丘的高度，我们也能确定没有山丘在这个范围内。毕竟山丘的最大高度是 100，而我们的范围从 101 开始。现在把我们的范围从 101–118 向下滑动到 100–117。这个 100–117 范围的成本小于 101–118 范围！这是因为 100 比 101 更靠近山丘。例如，考虑一座高度为 80 的山丘。将它升到高度 101 需要花费我们 21² = 441，但

只需 20² = 400 就能将它升到高度 100。这表明 101–118 不可能是最优范围。尝试它没有意义。

类似的逻辑解释了为什么尝试任何更高的范围（如 102–119、103–120 等等）是没有意义的。我们总是可以把这些范围向下滑动以降低成本。

总之，我们将恰好测试 101 个范围：0–17、1–18、2–19，依此类推，一直到 100–117。我们会记住最优范围的成本。让我们开始吧！

## 解决问题

我们将分两步求解，就像我们解决 Lifeguards 问题时一样。首先用一个函数来确定单个范围的成本。然后编写一个主程序，对每个范围调用一次该函数。

## 确定单个范围的成本

清单 9-3 给出了确定给定范围成本的函数代码。

```
MAX_DIFFERENCE = 17
MAX_HEIGHT = 100

def cost_for_range(heights, low, high):
    """
    heights is a list of hill heights.
    low is an integer giving the low end of the range.
    high is an integer giving the high end of a range.

    Return the cost of changing all heights of hills to be
    between low and high.
    """
    cost = 0
    for height in heights:
        if height < low:
            cost = cost + (low - height) ** 2
        elif height > high:
            cost = cost + (height - high) ** 2
    return cost
```

清单 9-3：为某一特定范围求解

```python
if result < min_cost:
    min_cost = result

output_file.write(str(min_cost) + '\n')

input_file.close()
output_file.close()
```

# 清单 9-4：主程序

我们首先读取山丘的数量，然后将每座山丘的高度读入 heights 列表。

我们使用 `min_cost` 变量来记住目前发现的最小成本。我们将 `min_cost` 设置为范围 0–17 的成本❶。然后，在一个 `range` for 循环❷中，我们尝试每一个其他范围的成本，只要找到更小的成本就更新 `min_cost`。循环结束后，我们输出找到的最小成本。

是时候把我们的代码提交给评测系统了。我们的完全搜索方案应该能在时限内轻松解决这个问题。

在下一个问题中，我们将看到一个直接使用完全搜索方案效率不够的例子。

## 概念检查

这里是对清单 9-4 中代码的一个修改建议。取这一行：

```python
for low in range(1, MAX_HEIGHT + 1):
```

并把它改为以下内容：

```python
for low in range(1, MAX_HEIGHT - MAX_DIFFERENCE + 1):
```

代码仍然正确吗？

+   A. 是
+   B. 否

> 答案：A。代码现在检查的最后一个范围是 83–100，所以我们必须论证那些不再检查的范围——84–101、85–102 等等——无关紧要。
> 
> 考虑范围 84–101。如果我们能论证范围 83–100 至少和 84–101 一样好，那么就没有理由检查范围 84–101。
> 
> 范围 84–101 包含了高度 101。但这是没有意义的：最高的山丘高度为 100，所以高度 101 甚至可以不存在。我们可以在不让范围变得更差的情况下移除 101。如果移除它，我们剩下范围 84–100。啊哈——但 100−84 只有 16，而我们允许差值达到 17。所以我们可以在左侧将范围扩展 1，得到范围 83–100。当然，像这样把范围变大不会让范围更差。它甚至可能让范围更好，因为现在它距离任何高度为 83 或更低的山丘都近了一个单位。
> 
> 我们从范围 84–101 出发，证明了范围 83–100 至少一样好。对于范围 85–102、86–103 等等，我们也能给出同样的论证。没有必要尝试任何高于 83–100 的范围！

在继续之前，你或许想尝试解决第 263 页“章节练习”中的练习 1 和练习 2。

## 问题 #23：奶牛棒球

为了结束这一章，我选择了一个问题，在这个问题中，我们需要将算法设计技能提升到超越完全搜索的水平。在你阅读题目时，要注意输入的量并不大。这通常意味着完全搜索算法会很有效。但这次不是，因为这种算法必须在这个输入上进行大量的搜索。难点归结为嵌套循环太多。为什么嵌套循环在这里会给我们带来麻烦？我们能做些什么？继续往下读！

这是 USACO 2013 年 12 月铜牌组竞赛问题“奶牛棒球”。

## 挑战

农夫约翰有 n 头奶牛。它们站成一排，每头位于一个不同的整数位置。它们在开心地互相扔棒球。

农夫约翰看着这些滑稽动作。他观察到奶牛 x 把球扔给它右侧的某头奶牛 y，然后奶牛 y 又把球扔给它右侧的某头奶牛 z。他还知道第二次扔球的距离至少是第一次扔球的距离，并且至多是第一次扔球距离的两倍。（例如，如果第一次扔球距离为 5，那么第二次扔球距离至少为 5，至多为 10。）

确定满足农夫约翰观察结果的奶牛三元组 (x, y, z) 的数量。

## 输入

从名为 baseball.in 的文件中读取输入。

输入由以下行组成：

+   - 一行包含 n，即奶牛的数量。n 在 3 到 1,000 之间。
+   - n 行，每行给出一头奶牛的位置。所有位置均唯一，每个位置在 1 到 100,000,000 之间。

## 输出

将输出写入名为 baseball.out 的文件。

输出满足农夫约翰观察结果的奶牛三元组的数量。

每个测试用例的时限为四秒。

## 使用三层嵌套循环

我们可以使用三层嵌套循环来考虑所有可能的三元组。我们先看一下代码，然后讨论它的效率。

## 代码

在第 3 章的“嵌套”一节中，我们学到了可以用两层嵌套循环遍历所有值对。做法如下：

```
>>> lst = [1, 9]
>>> for num1 in lst:
```





```
...   for num2 in lst:
...     print(num1, num2)
...
1 1
1 9
9 1
9 9
```

我们同样可以使用三个嵌套循环遍历所有三元组的值，如下所示：

```
>>> for num1 in lst:
...   for num2 in lst:
...     for num3 in lst:
...       print(num1, num2, num3)
...
1 1 1
1 1 9
1 9 1
1 9 9
9 1 1
9 1 9
9 9 1
9 9 9
```

使用这样的三个嵌套循环为解决 Cow Baseball 问题提供了一个起点。对于每个三元组，我们可以检查它是否与 Farmer John 的观察相匹配。代码请参见清单 9-5。

```
input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
  ① positions.append(int(input_file.readline()))

total = 0

② for position1 in positions:
   ③ for position2 in positions:
      first_two_diff = position2 - position1
   ④ if first_two_diff > 0:
      low = position2 + first_two_diff
      high = position2 + first_two_diff * 2
```

```
⑤ for position3 in positions:
    if position3 >= low and position3 <= high:
        total = total + 1

output_file.write(str(total) + '\n')

input_file.close()
output_file.close()
```

## Listing 9-5: Using three nested for loops

我们将所有的牛位置读入 positions 列表 ①。然后我们使用 for 循环 ②遍历列表中的所有位置。对于这些位置中的每一个，我们使用嵌套的 for 循环 ③遍历列表中的所有位置。此时，position1 和 position2 引用列表中的两个位置。我们需要第三个嵌套循环，是的，但还不是。我们首先需要计算 position1 和 position2 之间的差值，因为它告诉我们将要查找的 position3 的范围。

根据问题描述的要求，position2 必须在 position1 的右侧。如果是这样 ④，那么我们计算 position3 范围的下端和上端，并分别使用 low 和 high 存储它们。例如，如果 position1 是 1 而 position2 是 6，那么我们将计算 6 + 5 = 11 作为 low，6 + 5 * 2 = 16 作为 high。然后我们使用第三个嵌套 for 循环 ⑤遍历列表，查找介于 low 和 high 之间的位置。对于每一个这样的 position3，我们将 total 增加 1。

在三个嵌套循环之后，我们已经计算出三元组的总数。我们通过将该数字输出到输出文件来完成。

让我们在小测试用例上尝试我们的程序，以确保没有出现奇怪的情况。如下所示：

```
7
16
14
23
18
1
6
11
```

该测试用例的正确答案是 11。11 个满足条件的三元组如下：

- 14, 16, 18
- 14, 18, 23
- 1, 6, 16
- 1, 6, 14
- 1, 6, 11
- 1, 11, 23
- 6, 14, 23
- 6, 11, 16
- 6, 11, 18
- 11, 16, 23
- 11, 14, 18

好消息：我们的程序对此测试用例的输出为 11！它之所以如此，是因为它最终找到了每个满足条件的三元组。例如，在某个时刻，position1 将为 14，position2 将为 16，position3 将为 18。该三元组满足距离要求，因此我们的程序会将其计入 total。不要担心稍后会发生什么，当 position1 为 18，position2 为 16，position3 为 14 时。我们绝对不想计算那个，因为这些投掷不是向右的。我们没问题：if 语句 ④ 阻止了这些三元组的处理。

我们的程序是正确的。但正如你提交给评测时会看到的那样，它效率不足。对于这个问题，以及许多竞赛编程问题，前几个测试用例都很小——只有几头牛，几个救生员，或几个滑雪场。我们的程序应该能够在时间内解决这些问题。剩余的测试用例会让我们更接近可接受输入的极限地测试我们的程序。我们的程序无法在时间内解决这些问题。它太慢了。

## Efficiency of Our Program

要理解我们的程序为何如此缓慢，有助于思考它必须遍历的三元组数量。回顾一下我们刚刚研究的测试用例，其中有七头牛。我们的程序会检查多少个三元组呢？对于第一头牛，有七种选择：16、14、23 等等。第二头牛也有七种选择，第三头牛也有七种选择。将这些相乘，我们看到我们的程序检查了 7 * 7 * 7 = 343 个三元组。

如果我们有八头牛而不是七头呢？那么我们的程序将检查 8 * 8 * 8 = 512 个三元组。

我们可以给出一个适用于任意牛数量的三元组数量的表达式。让我们用 n 表示牛的数量；它可能是 7、8、50、1,000，等等，具体取决于测试用例。然后我们可以说，我们的程序检查的三元组数量是 n * n * n，即 n³。

我们可以将任意数量的牛代入 n 来确定我们检查的三元组数量。例如，我们可以验证七头牛的三元组数量为 7³ = 343，八头牛的三元组数量为 8³ = 512。这些数字——343 和 512——都很小。任何计算机在几毫秒内检查这么多三元组都绰绰有余。作为保守的指导，你可以认为 Python 程序每秒能够检查或执行约 5,000,000 个操作。该问题的时限为每个测试用例四秒，因此我们能够检查大约 20,000,000 个三元组。

让我们代入更大的 n 看看会发生什么。对于 50 头牛，我们有 50³ = 125,000 个三元组。没什么大不了的：检查 125,000 个东西对今天的计算机来说很容易。对于 100 头牛，我们有 100³ = 1,000,000 个三元组。同样，没问题。我们可以在不到一秒的时间内检查一百万个东西。对于 200 头牛，我们有 200³ = 8,000,000 个三元组。我们对于四秒来说仍然可以，但我希望你开始有点担心了。这里的三元组数量增长得相当快，而我们才只有 200 头牛。记住，我们需要能够支持多达 1,000 头牛。

对于 400 头牛，我们有 400³ = 64,000,000 个三元组。这对于我们在四秒内处理来说太多了。为了雪上加霜，让我们尝试 1,000 头牛，即我们将获得的最大值。对于 1,000 头牛，我们有 1,000³ = 1,000,000,000 个三元组。那是一百亿。不行。无论如何，我们都不可能在四秒内检查这么多三元组。我们需要使我们的程序更高效。

## Sorting First

排序在这里很有帮助。让我们看看如何使用排序，然后讨论我们 resulting 解决方案的效率。

## The Code

我们的牛位置可以按任意顺序出现——根据问题描述，绝不能保证它们已排序。不幸的是，这导致我们的程序检查了许多不可能满足要求的三元组。例如，检查三元组 18、16、14 是没有意义的，因为数字不是递增的。如果我们在开始时对牛位置进行排序，那么我们就可以避免 ever 检查这些顺序错误的<|message|> 三元组。

排序还有另一个好处。假设 position1 引用某个牛位置，position2 引用另一个。对于这对位置，我们知道我们关心的 position3 的最小值和最大值。我们可以利用位置已排序这一事实来减少我们需要为此范围检查的值的数量。在继续之前，想想为什么会这样。我们如何利用位置已排序这一事实来查看更少的值？

准备好了的话，请参见清单 9-6，我们使用排序的代码。

```
input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
    positions.append(int(input_file.readline()))

positions.sort()

total = 0

for i in range(n):
    for j in range(i + 1, n):
        first_two_diff = positions[j] - positions[i]
        low = positions[j] + first_two_diff
        high = positions[j] + first_two_diff * 2

        left = j + 1
        while left < n and positions[left] < low:
            left = left + 1

        right = left
        while right < n and positions[right] <= high:
            right = right + 1

        total = total + right - left
```





```
output_file.write(str(total) + '\n')

input_file.close()
output_file.close()
```

## Listing 9-6. Using sorting

在开始寻找三元组之前，我们先对位置进行排序 ❶。

我们的第一个循环使用循环变量 `i` 遍历所有位置 ❷。这次使用的是 range 循环，而不是 for-each 循环，这样我们才能跟踪当前所在的索引。这很有用，因为我们可以用 `i + 1` 的值作为第二个循环的起始索引 ❸。第二个循环现在永远不会浪费时间去查看第一个位置左侧的位置。

接下来我们计算第三个位置的值范围的低端和高点。

与其每次找到合适的第三个位置就把 total 增加 1，我们可以改为先找到合适位置的左右边界，然后一次性增加 total。我们之所以能这样做，是因为位置列表是有序的。我们使用 while 循环来找到每个边界。第一个 while 循环找到左边界 ❹。它会一直进行下去，只要位置小于 low。完成后，left 将是位置大于或等于 low 的最左索引。第二个 while 循环找到右边界 ❺。它会一直进行下去，只要位置小于或等于 high。完成后，right 是位置大于 high 的最左索引。从 left 到 right 但不包括 right 的每个位置都可以作为涉及索引 i 和 j 位置的三元组的第三个位置。我们将 right - left 加到 total 上以计入这些位置 ❻。

该程序中的两个 while 循环相当棘手。让我们通过一个示例来确保我们完全了解它们在做什么。我们将使用以下位置列表；它们与上一节中使用的相同，但已排序：

```
[1, 6, 11, 14, 16, 18, 23]
```

假设 i 为 1 且 j 为 2，因此候选三元组中的两个位置为 6 和 11。对于第三个位置，我们因此寻找大于或等于 16 且小于或等于 21 的位置。第一个 while 循环会将 left 设置为 4，即位置大于或等于 16 的最左索引。第二个 while 循环会将 right 设置为 6，即位置大于 21 的最左索引。left 从 right 中减去，我们得到 6 − 4 = 2，这意味着涉及位置 6 和 11 的三元组有两种。在继续之前，我鼓励你确认这些 while 循环在“特殊”情况下也能正常工作，例如当没有合适的第三个位置或只有一个合适的第三个位置时。

我们在本节中取得了长足的进展。这里的代码肯定比我们在 Listing 9-5 中给出的代码更高效。然而，它仍然不够高效。如果你提交给评测，你会发现它并没有比上次更进一步。它在大多数测试用例上仍然会超时。

## Efficiency of Our Program

我们程序的问题在于找到第三个位置仍然可能需要很长时间。那些 while 循环仍然存在一些低效。我可以用一个新的位置列表来证明这一点，即从 1 到 32 的位置。

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,
17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32]
```

让我们关注 i 为 0 且 j 为 7 的情况；这些是位置 1 和 8。对于第三个位置，我们寻找大于或等于 15 且小于或等于 22 的位置。为了找到 15，第一个 while 循环会向右扫描，每次一个位置。它扫描了 9，然后是 10，然后是 11，然后是 12，然后是 13，然后是 14，最后是 15。然后第二个 while 循环接管，同样进行大量扫描，每次一个位置，一直扫描到找到 23 为止。

每个 while 循环实现的是所谓的线性搜索。线性搜索是一种逐个值搜索集合的技术。这是一项大量工作，需要扫描所有这些值！而且还有许多其他 i 和 j 的值会导致类似的工作量。例如，试着追踪 i 为 0 且 j 为 8 时，或 i 为 1 且 j 为 11 时会发生什么。

我们如何改进？如何避免扫描列表的大块内容来寻找适当的 left 和 right 索引？

假设我给你一本包含一千个已排序整数的书，每行一个整数。我要求你找到第一个大于或

## The random Module

```
>>> import random
```

里面有什么？要了解，可以使用 dir(random)：

```
>>> dir(random)
['BPF', 'LOG4', 'NV_MAGICCONST', 'RECIP_BPF', 'Random', 'SG_MAGICCONST',
 'SystemRandom', 'TWOPI', 'betavariate', 'choice', 'choices', 'expovariate',
 'gammavariate', 'gauss', 'getrandbits', 'getstate',
 'lognormvariate', 'normalvariate', 'paretovariate',
 'randint', 'random', 'randrange', 'sample', 'seed',
 'setstate', 'shuffle', 'triangular', 'uniform',
 'vonmisesvariate', 'weibullvariate']
```

random 模块提供的一个函数是 randint。我们传入一个范围的低端和高点，Python 会给我们该范围内的一个随机整数（包括两个端点）。

但我们不能直接像普通函数那样调用它。如果我们尝试，会得到一个错误：

```
>>> randint(2, 10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'randint' is not defined
```

我们需要告诉 Python randint 函数位于 random 模块中。为此，我们在 randint 前加上模块名和点，如下所示：

```
>>> random.randint(2, 10)
7
>>> random.randint(2, 10)
10
>>> random.randint(2, 10)
6
```

要获取 randint 函数的帮助，可以输入 help(random.randint)：

```
>>> help(random.randint)
Help on method randint in module random:

randint(a, b) method of random.Random instance
    Return random integer in range [a, b], including both end points.
```

random 模块中的另一个有用函数是 choice。我们传入一个序列，它会随机返回其一个值：

```
>>> random.choice(['win', 'lose'])
'lose'
>>> random.choice(['win', 'lose'])
'lose'
>>> random.choice(['win', 'lose'])
'win'
```

如果我们经常使用模块中的少量函数，每次都输入模块名和点会很麻烦。导入这些函数还有另一种方法，可以让我们像其他非模块函数一样调用它们。这就是我们如何只导入 randint 函数：

```
>>> from random import randint
```

现在我们可以不加 random. 前缀调用 randint：

```
>>> randint(2, 10)
10
```

如果我们需要 randint 和 choice，可以同时导入它们：

```
>>> from random import randint, choice
```

我们不会在本书中这样做，但我们可以创建包含我们喜欢的任何函数的自己的模块。例如，如果我们设计了一些与玩游戏相关的 Python 函数，我们可以将它们全部放在名为 *game_functions.py* 的文件中。然后我们可以使用 import game_functions 导入该模块，然后访问其中的函数。

我们在这本书中编写的 Python 程序并不是设计为作为模块导入的。原因是它们一开始运行就会读取输入。模块不应该这样做。相反，模块应该在调用其函数之前等待再做任何事情。random 模块就是一个表现良好的模块的例子：只有在我们要求时它才会开始给我们随机事物。

## The bisect Module

现在我们准备玩转二分搜索。在 Listing 9-6 中，我们有两个 while 循环。它们很慢，所以我们想摆脱它们。为此，我们将用二分搜索函数的调用替换每一个：第一个 while 循环用 bisect_left，第二个用 bisect_right。

这两个函数都在 bisect 模块中。让我们导入它们：

```
>>> from bisect import bisect_left, bisect_right
```

让我们先讨论 bisect_left。我们通过提供一个从小到大排序的列表和一个值 x 来调用它。它返回列表中大于或等于 x 的最左值的索引。

如果该值在列表中，我们会得到其最左出现位置的索引：

```
>>> bisect_left([10, 50, 80, 80, 100], 10)
0
>>> bisect_left([10, 50, 80, 80, 100], 80)
2
```

如果该值不在列表中，则我们会得到第一个大于该值的索引：

```
>>> bisect_left([10, 50, 80, 80, 100], 15)
1
>>> bisect_left([10, 50, 80, 80, 100], 81)
4
```

如果我们搜索大于列表中每个值的某物，我们会得到列表的长度：

```
>>> bisect_left([10, 50, 80, 80, 100], 986)
5
```

让我们在“Sorting First”中本章早些时候的七个位置列表上使用 bisect_left。我们将找到第一个大于或等于 16 的位置的索引：

```
>>> positions = [1, 6, 11, 14, 16, 18, 23]
>>> bisect_left(positions, 16)
4
```

完美：这正是我们需要替换 Listing 9-6 中第一个 while 循环的东西。

要替换第二个 while 循环，我们将使用 bisect_right 而不是 bisect_left。我们调用 bisect_right 的方式与调用 bisect_left 相同：传入一个已排序的列表和一个值 x。与返回列表中大于或等于 x 的最左值的索引不同，它返回大于 x 的最左值的索引。





让我们比较 bisect_left 和 bisect_right。对于列表中存在的值，bisect_right 返回的索引大于 bisect_left 返回的索引：

```
>>> bisect_left([10, 50, 80, 80, 100], 10)
0
>>> bisect_right([10, 50, 80, 80, 100], 10)
1
>>> bisect_left([10, 50, 80, 80, 100], 80)
2
>>> bisect_right([10, 50, 80, 80, 100], 80)
4
```

对于列表中不存在的值，bisect_left 和 bisect_right 返回相同的索引：

```
>>> bisect_left([10, 50, 80, 80, 100], 15)
1
>>> bisect_right([10, 50, 80, 80, 100], 15)
1
>>> bisect_left([10, 50, 80, 80, 100], 81)
4
>>> bisect_right([10, 50, 80, 80, 100], 81)
4
>>> bisect_left([10, 50, 80, 80, 100], 986)
5
>>> bisect_right([10, 50, 80, 80, 100], 986)
5
```

让我们在“Sorting First”中本章前面提到的七个位置列表上使用 bisect_right。我们将找到大于 21 的最左侧位置的索引：

```
>>> positions = [1, 6, 11, 14, 16, 18, 23]
>>> bisect_right(positions, 21)
6
```

就这样：这就是我们可以用它来替换 Listing 9-6 中的第二个 while 循环的东西。

使用这些微小的例子很难体会到二分搜索惊人的速度。是时候来点实际的了。我们将在长度为 1000000 的列表中 100 万次搜索最右值。运行这段代码时不要移开视线。你可能会错过它。

```
>>> lst = list(range(1, 1000001))
>>> for i in range(1000000):
...    where = bisect_left(lst, 1000000)
...
```

在我电脑上，大约需要一秒钟。你可能在想，如果用列表索引方法调用替换二分搜索会怎样。如果你试试，你会 literally 等待数小时代码才能运行。这是因为 index，就像 in 运算符一样，通过列表进行线性搜索。参见第 8 章中的“Efficiency of Searching a List”了解更多信息。它没有保证列表是有序的，所以它不能执行闪电般的二分搜索。它必须逐个遍历值，将它们中的每一个与我们正在搜索的值进行比较。如果你有一个已排序的列表并且想要在其中查找值，二分搜索是无可匹敌的。

## Solving the Problem

我们准备好使用二分搜索解决 Cow Baseball。代码见 Listing 9-7。

```
① from bisect import bisect_left, bisect_right

input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
    positions.append(int(input_file.readline()))

positions.sort()

total = 0

for i in range(n):
    for j in range(i + 1, n):
        first_two_diff = positions[j] - positions[i]
        low = positions[j] + first_two_diff
        high = positions[j] + first_two_diff * 2
        ② left = bisect_left(positions, low)
        ③ right = bisect_right(positions, high)
        total = total + right - left

output_file.write(str(total) + '\n')
```

```
input_file.close()
output_file.close()
```

### Listing 9-7: *Using binary search*

首先，我们从 bisect 模块导入 bisect_left 和 bisect_right 函数，以便我们可以调用它们 ❶。与 Listing 9-6 相比，唯一的其他区别是我们现在使用 bisect_left ❷ 和 bisect_right ❸ 而不是 while 循环。

如果你现在将我们的代码提交给评测，你应该在时限内通过所有测试用例。

我们在本节中遵循的弧线是解决难题时典型的要求。我们可能从一个正确但遗憾地太慢的完整搜索解决方案开始，未能满足评测的时限。然后我们做出改进，使我们远离完整搜索，朝着更精炼的方法前进。

## CONCEPT CHECK

假设我们从 Listing 9-7 开始并用 bisect_left 替换 bisect_right。也就是说，我们取这一行：

```
right = bisect_right(positions, high)
```

并将其更改为以下内容：

```
right = bisect_left(positions, high)
```

程序仍然会产生正确的答案吗？

-   A. 它总是产生正确的答案，就像以前一样。
-   B. 它有时会产生正确的答案；取决于测试用例。
-   C. 它永远不会产生正确的答案。

答案：B。存在测试用例使得修改后的代码确实会产生正确的答案。这里有一个：

```
3
2
4
9
```

正确答案是 0，这就是我们的程序产生的结果。

要小心，因为还有其他测试用例使得修改后的代码会产生错误答案。这里有一个：

```
3
2
4
8
```

正确答案是 1，但我们的程序产生 0。当 i 为 0 且 j 为 1 时，程序本应将 left 设置为 2 并将 right 设置为 3。不幸的是，使用 bisect_left 导致 right 被设置为 2，因为索引 2 处的位 置是大于或等于 8 的最左侧位置。

鉴于这个反例，你可能会惊讶地知道，有一种方法可以使用 bisect_left 而不是 bisect_right。要做到这一点，我们需要在调用 bisect_left 时更改我们搜索的内容。如果你好奇，试试看！

## Summary

在本章中，我们学习了完整搜索算法，这些算法通过搜索所有选项来找到最佳选项。为了确定我们应该解雇哪位救生员，我们尝试解雇每位救生员并选择最好的一个。为了确定修复滑雪山的最低成本，我们尝试所有有效范围并选择最好的一个。为了确定相关奶牛三元组的数量，我们检查每个三元组并添加满足要求的那些。

有时，完整搜索算法本身就足够高效了。我们用未修饰的完整搜索代码解决了 Lifeguards 和 Ski Hills 问题。但其他时候，我们需要使完整搜索算法更高效。我们在解决 Cow Baseball 时通过用更快的二分搜索替换完整搜索 while 循环来做到这一点。

程序员和计算机科学家如何讨论效率？你如何知道算法是否足够高效？而你能否避免实现那些根本太慢的算法？第 10 章在等待。

## Chapter Exercises

这里有一些练习供你尝试。对于每个练习，使用完整搜索。如果你的解决方案效率不够，思考如何使其更高效，同时仍能产生正确答案。

对于每个练习，请仔细检查该问题来自的评测：有些在 DMOJ 评测上，而其他在 USACO 评测上。

-   1. USACO 2019 January Bronze Contest problem Shell Game
-   2. USACO 2016 US Open Bronze Contest problem Diamond Collector
-   3. DMOJ problem coci20c1p1, Patkice
-   4. DMOJ problem ccc09j2, Old Fishin’ Hole
-   5. DMOJ problem ecoo16r1p2, Spindie
-   6. DMOJ problem cco96p2, SafeBreaker
-   7. USACO 2019 December Bronze Contest problem Where Am I
-   8. USACO 2016 January Bronze Contest problem Angry Cows
-   9. USACO 2016 December Silver Contest problem Counting Haybales
-   10. DMOJ problem crci06p3, Firefly

## Notes

Lifeguards 最初来自 USACO 2018 January Bronze Contest。Ski Hills 最初来自 USACO 2014 January Bronze Contest。
