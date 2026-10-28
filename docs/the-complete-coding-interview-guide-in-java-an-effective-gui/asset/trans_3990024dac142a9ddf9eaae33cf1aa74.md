

## 无符号右移 [>>>]

无符号右移，或逻辑右移 [>>>]，接受两个操作数。无符号右移获取第一个操作数（左操作数）的位模式，并将其向右移动第二个操作数（右操作数）指定的位数。MSB 被设为 0。这意味着，对于正数，带符号右移和无符号右移返回相同的结果，而负数总是变成正数。

例如，-75 >>> 1 的结果如下（-75 是一个 8 位整数，其中符号位是 MSB）：

![](img/6709a71136a6b327c831c6c15aff2f6a_265_0.png)

图 9.6 – 无符号右移

> **重要说明**
> 
> 移动的位数会自动缩减为模 32；也就是说，23 >>> 35 等价于 23 >>> (35 % 32)，等价于 23 >>> 3。

现在你已经对位移运算符有了一个概念，是时候来攻克更多技巧了。

## 技巧与窍门

操作位在处理位运算符时需要极高的技巧，并且了解一些技巧和窍门。你在本章前面已经看到了一些技巧。现在，让我们再以项目符号列表的形式添加一些：

- 如果我们对一个数进行偶数次 XOR，则结果为 0（x ^ x = 0; x ^ x ^ x ^ x = (x ^ x) ^ (x ^ x) = 0 ^ 0 = 0）。
- 如果我们对一个数进行奇数次 XOR，则结果为该数（x ^ x ^ x = (x ^ (x ^ x)) = (x ^ 0) = x; x ^ x ^ x ^ x ^ x = (x ^ (x ^ x) ^ (x ^ x)) = (x ^ 0 ^ 0) = x）。

> > 我在代码审阅中发现的一些问题

昨天,团队在 review 我提交的 PR。有人提了一个很尖锐的问题:虽有成形的方案,但我们的 coding agent 只依赖静态分析,没有真正执行代码去验证行为。

这个反馈让我想了很多。coding agent 不能只是"读代码",更需要"运行代码"去验证假设。而当我们提及"让 agent 运行代码",不仅仅是执行几个测试那么简单,而是要面对 Linux 容器环境中的权限、安全隔离,以及环境一致性等一系列复杂的问题。

为了模拟这些挑战,我设计了一个安全的演练环境,利用 Docker-in-Docker 构建一个隔离的 Linux 沙盒,并把沙盒工具化,让 coding agent 能:

- 创建隔离的 Linux 容器
- 在容器内执行任意命令
- 支持文件读写、安装依赖、运行测试

下面,我把构建过程、关键决策和遇到的实际问题完整记录下来。

## 项目概览

目标是构建一个沙盒工具,与 coding agent 集成(coding agent 通过 Tool Calling 调用该工具),在容器内安全地抽象和验证代码行为。

整体架构如下:

![](img/6709a71136a6b327c831c6c15aff2f6a_266_0.png)

## 核心设计原则

- **隔离性**: 每个沙盒运行在独立的 Docker 容器中,互不干扰
- **安全性**: Agent 没有宿主机权限,仅限容器内操作
- **可重复性**: 环境配置通过 Dockerfile 和脚本固化,保证环境一致
- **可观测性**: 所有命令输出和操作日志都会被捕获

## Tool Calling 接口设计

sandbox 以 Tool 的形式对 agent 开放,关键接口如下:

```

```json
{
  "type": "function",
  "function": {
    "name": "sandbox_exec",
    "description": "在隔离的容器中执行命令并返回输出",
    "parameters": {
      "type": "object",
      "properties": {
        "image": { "type": "string" },
        "cmd": { "type": "array" },
        "timeout": { "type": "number" }
      }
    }
  }
}
```

```

So, we cleared the bits between MSB and k=6. Generally speaking, we need a bit-mask that contains 0s between the MSB and k (inclusive) and 1s between k (exclusive) and LSB. We can do this by left shifting the bits of 1 with k positions (for example, for k=6, we obtain 1000000) and subtracting 1. This way, we obtain the needed bit-mask, 0111111. So, in terms of code, we have the following:

```

public static int clearFromMsb(int n, int k) {

    return n & ((1 << k) - 1);

}

```

How about clearing the bits between the given k and the LSB? Let me show you the code:

```

public static int clearFromPosition(int n, int k) {

    return n & ~((1 << k) - 1);

}

```

Now, take your time and dissect this solution. Moreover, we can replace this solution with this one: n & (-1 << (k + 1)).

Again, use a paper and a pen and take it step by step. The complete application is called ClearBits.

## 编程挑战 4 - 在纸上求二进制和

**问题：** 考虑若干个正的 32 位整数。拿起笔和纸，展示你如何求出它们的二进制表示之和。

**说明：** 这并不是一个严格意义上的编程挑战，但了解这一点很重要。

**解答：** 求二进制数之和可以有多种方法。一种简单的方法如下：

1. 求当前列的所有位之和（第一列是 LSB 列）。
2. 将结果转换为二进制（例如，通过连续除以 2）。
3. 保留最右边的位作为结果。
4. 将剩余的位进位到剩余的列（每列一位）。
5. 进入下一列并从步骤 1 重复。

## 编程挑战 8 - 在纸上做二进制减法

问题：考虑两个正的 32 位整数 q 和 p。拿起纸和笔，展示你如何减去这两个数的二进制表示（q-p）。

说明：这并不是一个严格意义上的编程挑战，但了解这一点很重要。

解答：二进制减法可以简化为计算 0 减 1。主要我们知道 1 减 1 等于 0，0 减 0 等于 0，1 减 0 等于 1。要计算 0 减 1，我们必须执行以下步骤：

1. 从当前列开始，向左列搜索直到找到一个 1 位。
2. 借用该位并将其放在前一列作为两个 1 的值。
3. 再从前一列借用其中一个 1 的值作为另外两个 1。
4. 对每个列重复步骤 3，直到到达当前列。
5. 现在，我们可以执行计算。
6. 如果我们再次遇到 0 减 1，则从步骤 1 重复此过程。

让我们做 124 (1111100) - 29 (011101) = 95 (1011111)。

下图表示我们计算的结果：

![](img/6709a71136a6b327c831c6c15aff2f6a_276_0.png)

图 9.16 – 减去二进制数

现在，让我们一步步来看：

1. 从第 0 列开始，即从 0 减 1。我们在左侧列中搜索直到找到一个 1 位。我们在第 2 列找到它（该位对应于 2²=4）。我们借用该位到第 1 列并将其用作两个 1 的值（换句话说，两个 2 是 2¹+2¹）。我们从第 1 列借用其中一个 1 的值（这是 2¹=2）到第 0 列并将其用作另外两个 1 的值（换句话说，两个 1 是 2⁰+2⁰）。现在，我们可以进行计算 2 减 1 等于 1。我们写下 1 并继续到第 1 列。

