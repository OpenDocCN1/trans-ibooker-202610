

```
}
}
}
}

    return doors;
}
```

完整的应用程序名为 *VisitToggle100Doors*。

## 编程挑战 4 – 8 支队伍

**Amazon, Google, Adobe**

**问题：** 假设有一个比赛，有 8 支队伍。每支队伍与其他队伍各比赛两次。从所有这些队伍中，只有 4 支进入半决赛。一支队伍需要赢多少场比赛才能晋级半决赛？

**解决方案：** 设队伍为 T1、T2、T3、T4、T5、T6、T7 和 T8。如果 T1 与 T2...T8 比赛，它们将进行 7 场比赛。由于每支队伍必须与其它队伍各比赛两次，我们有 8*7=56 场比赛。如果在每场比赛中，一支队伍可以赢得 1 分，那么我们有 56 分分配给 8 支队伍。

让我们考虑最坏情况。T0 输掉所有比赛。这意味着 T0 得到 0 分。另一方面，T1 在对阵 T0 时赢得 2 分并输掉所有其它比赛，T2 在对阵 T0 和 T1 时赢得 4 分并输掉所有其它比赛，T3 在对阵 T0、T1 和 T2 时赢得 6 分并输掉所有其它比赛，依此类推。T4 赢得 8 分，T5 赢得 10 分，T6 赢得 12 分，T7 赢得 14 分。因此，一支队伍赢得所有比赛可赢得 14 分。最后四支队伍（进入半决赛的队伍）共赢得 8+10+12+14=44 分。因此，一支队伍如果获得至少 44/4=11 分就可以确保晋级半决赛。

## 编程挑战 5 – 寻找质因数仅为 3、5 和 7 的第 k 个数

Adobe, Microsoft

设计一个算法来寻找仅有质因数 3、5 和 7 的第 k 个数。

拥有一个仅有质因数 3、5 和 7 的数列意味着一个如下所示的数列：1, 3, 5, 7, 9, 15, 21, 25，等等。或者，更具启发性地，可以写成如下形式：1, 1*3, 1*5, 1*7, 3*3, 3*5, 3*7, 5*3, 3*3*5, 5*7, 3*3*5, 7*7，等等。

通过这种启发式表示，我们可以看到我们可以初始将值 1 插入到一个列表中，而其余元素必须计算得到。理解确定其余元素的算法最简单的方法是查看实现本身，所以让我们看一下：

```
public static int kth(int k) {
	int count3 = 0;
	int count5 = 0;
	int count7 = 0;
	List<Integer> list = new ArrayList<>();
	list.add(1);
	while (list.size() <= k + 1) {
		int m = min(min(list.get(count3) * 3, list.get(count5) * 5), list.get(count7) * 7);
		list.add(m);
		if (m == list.get(count3) * 3) {
			count3++;
		}
		if (m == list.get(count5) * 5) {
			count5++;
		}
		if (m == list.get(count7) * 7) {
			count7++;
		}
	}
	return list.get(k);
}
```

```
if (m == list.get(count7) * 7) {
    count7++;
}
}

return list.get(k - 1);
}
```

我们也可以通过三个队列提供一种实现。该算法的步骤如下：
- 1. 初始化一个整数，minElem=1。
- 2. 初始化三个队列；即 queue3、queue5 和 queue7。
- 3. 从 1 循环到给定的 k-1：
  - a. 将 minElem*3、minElem*5 和 minElem*7 分别插入 queue3、queue5 和 queue7。
  - b. 将 minElem 更新为 min(queue3.peek, queue5.peek, queue7.peek)。
  - c. 如果 minElem 是 queue3.peek，则执行 queue3.poll。
  - d. 如果 minElem 是 queue5.peek，则执行 queue5.poll。
  - e. 如果 minElem 是 queue7.peek，则执行 queue7.poll。
- 4. 返回 minElem。

完整的应用程序名为 KthNumber357。它包含本节中介绍的两种解决方案。

## 编程挑战 6 - 计数解码数字序列

Amazon, Microsoft, Flipkart

问题：假设 A 为 1，B 为 2，C 为 3，... Z 为 26。对于任意给定的数字序列，编写一段代码来计算可能的解码数（例如，1234 可以解码为 1 2 3 4、12 3 4 和 1 23 4，这意味着分别对应 ABCD、LCD 和 AWD）。给定的数字序列有效，如果它包含 0 到 9 的数字。不允许前导 0，不允许额外尾随 0，且不允许两个或更多连续的 0。

**解决方案**：该问题可以通过递归或动态规划解决。两种技术都在 *第 8 章，递归和动态规划* 中介绍过。所以，让我们来看一下针对 *n* 位数字序列的递归算法：
- 1. 将解码总数初始化为 0。
- 2. 从给定的数字序列末尾开始。
- 3. 如果最后一位数字不是 0，则对 (*n*-1) 位数字应用递归，并使用结果更新解码总数。
- 4. 如果最后两位数字代表一个小于 27 的数（因此是一个有效字符），则对 (*n*-2) 位数字应用递归，并使用结果更新解码总数。

就代码而言，我们有以下内容：

```
public static int decoding(char[] digits, int n) {

    // base cases
    if (n == 0 || n == 1) {
        return 1;
    }

    // if the digits[] starts with 0 (for example, '0212')
    if (digits == null || digits[0] == '0') {
        return 0;
    }

    int count = 0;

    // If the last digit is not 0 then last
    // digit must add to the number of words
    if (digits[n - 1] > '0') {
        count = decoding(digits, n - 1);
    }

    // If the last two digits represents a number smaller
    // than or equal to 26 then consider last two digits
    // and call decoding()
```

```
if (digits[n - 2] == '1'
    || (digits[n - 2] == '2' && digits[n - 1] < '7')) {
    count += decoding(digits, n - 2);
}

return count;
}
```

此代码运行时间为指数级。但我们可以通过类似的非递归算法应用动态规划将运行时间降低到 O(n)，如下所示：

```
public static int decoding(char digits[]) {

    // if the digits[] starts with 0 (for example, '0212')
    if (digits == null || digits[0] == '0') {
        return 0;
    }

    int n = digits.length;

    // store results of sub-problems
    int count[] = new int[n + 1];

    count[0] = 1;
    count[1] = 1;

    for (int i = 2; i <= n; i++) {
        count[i] = 0;

        // If the last digit is not 0 then last digit must
        // add to the number of words
        if (digits[i - 1] > '0') {
            count[i] = count[i - 1];
        }

        // If the second last digit is smaller than 2 and
        // the last digit is smaller than 7, then last
```

```
// two digits represent a valid character
if (digits[i - 2] == '1' || (digits[i - 2] == '2'
    && digits[i - 1] < '7')) {
    count[i] += count[i - 2];
}
}

return count[n];
}
```

此代码运行时间为 O(n)。完整的应用程序名为 *DecodingDigitSequence*。

## 编程挑战 7 – ABCD

**问题：** 找到一个四位数类型 ABCD，使得乘以 4 后得到 DCBA。

**解决方案：** 这类问题通常相当困难。在这种情况下，我们必须使用一些数学来解决它。

让我们从一些简单的不等式开始：
- 1 <= A <= 9 （A 不能为零，因为 ABCD 是一个四位数）
- 0 <= B <= 9
- 0 <= C <= 9
- 4 <= D <= 9 （D 必须至少为 4* A，所以它应该至少为 4）

接下来，我们可以假设我们的数字 ABCD 写成 1000A + 100B + 10C + D。根据问题陈述，我们可以将 ABCD 乘以 4 得到 DCBA，可以写成 1000D + 100C + 10B + A。

符合 4 的整除性，BA 是一个能被 4 整除的两位数。现在，较大的 ABCD 是 2499，因为大于 2499 的数乘以 4 后结果将是五位数。

接下来，A 可以是 1 和 2。然而，如果 BA 是一个能被 4 整除的两位数，则 A 必须为偶数，所以它必须是 2。

继续这种逻辑，这意味着 D 要么是 8 要么是 9。然而，由于 D 乘以 4 的个位为 2，D 必须是 8。

此外，4000A + 400B + 40C + 4D = 1000D + 100C + 10B + A。由于 A=2 且 D=8，这可以写成 2C-13B=1。B 和 C 只能是 [1, 7] 中的一位整数，但 B 必须为奇数，因为 BA 是一个能被 4 整除的两位数。由于最大可能数为 2499，这意味着 B 可以是 1 或 3。

因此，结果是 2178，因为 2178*4=8712，所以 ABCD*4=DCBA。

我们也可以使用暴力方法找到这个数。以下代码不言自明：

```
public static void find() {

    for (int i = 1000; i < 2499; i++) {

        int p = i;
        int q = i * 4;

        String m = String.valueOf(p);
        String n = new StringBuilder(String.valueOf(q))
            .reverse().toString();

        p = Integer.parseInt(m);
        q = Integer.parseInt(n);

        if (p == q) {
            System.out.println("\n\nFound: " + p + " : " + (q * 4));
            break;
        }
    }
}
```

完整的应用程序名为 Abcd。

## 编程挑战 8 – 矩形重叠

Amazon, Google, Microsoft

问题：假设你被给出两个矩形。编写一段代码片段，如果这些矩形重叠（也称为碰撞或相交），则返回 true。

