

3. 我们为此函数的代码也将与 `LCS_BruteForce()` 非常相似，只是我们将通过递归遍历两个字符串的前缀（从完整字符串开始）来反转逻辑，并在每一步将结果存储在我们的 **memo** 表中：

```
// Base case - LCS is always zero for empty strings
if(i == 0 || j == 0)
{
    return 0;
}

// Have we found a result for the prefixes of the two strings?
if(memo[i - 1][j - 1] != UNKNOWN)
{
    // If so, return it
    return memo[i - 1][j - 1];
}

// Are the last characters of A's prefix and B's prefix equal?
if(A[i-1] == B[j-1])
{
    // LCS for this state is equal to 1 plus the LCS of the prefixes of A
    // and B, both reduced by one character
    memo[i-1][j-1] = 1 + LCS_Memoization(A, B, i-1, j-1, memo);

    // Return the cached result
    return memo[i-1][j-1];
}

// If the last characters are not equal, LCS for this state is equal to
// the maximum LCS of A's prefix reduced by one character and B's prefix, and
// B's prefix reduced by one character and A's prefix

memo[i-1][j-1] = max(LCS_Memoization(A, B, i-1, j, memo),
                     LCS_Memoization(A, B, i, j-1, memo));

return memo[i-1][j-1];
```

4. 现在，让我们重新定义我们的 **main()** 函数以执行这两种方法并显示每种方法所花费的时间：

```
int main()
{
    string A, B;
    cin >> A >> B;

    int tests = 2;

    clock_t timer = clock();

    for(int i = 0; i < tests; i++)
    {
        int LCS;

        switch(i)
        {
            case 0:
            {
                LCS = LCS_BruteForce(A, B, 0, 0, {});

                #if DEBUG
                    PrintSubsequences(A, B);
                #endif
                break;
            }
            case 1:
            {
                vector<vector<int>> memo(A.size(), vector<int>(B.size(), UNKNOWN));
                LCS = LCS_Memoization(A, B, A.size(), B.size(), memo);
                break;
            }
        }
        cout << "Length of the longest common subsequence of " << A << " and " << B << " is: " << LCS << ends;
    }
}
```

```
GetTime(timer, types[i]);

    cout << endl;
}
    return 0;
}
```

5. 现在，让我们尝试对两个新字符串 ABCABDBEFBA 和 ABCBEFBEAB 使用我们的两种算法。你的程序输出应类似于以下内容：

```
SIZE = 3
    ABC_______ ABC_______
SIZE = 4
    ABC_B_____ ABCB_____
    ABC_B_____ ABC___B__
    ABC_B_____ ABC_____B
    ABC___B___ ABC_____B
    ABC____E__ ABC___E__
    ABC_____B_ ABC___B__
    ABC_____B_ ABC_____B
    ABC______A ABC_____A_
SIZE = 5
    ABCAB_____ ABC____AB
    ABC_B_B___ ABCB_____B
    ABC_B__E__ ABCB____E_
    ABC_B___B_ ABCB__B__
    ABC_B___B_ ABCB_____B
    ABC_B____A ABCB____A_
    ABC_B_B___ ABC___B__B
    ABC_B__E__ ABC___BE__
    ABC_B___B_ ABC___B__B
    ABC_B____A ABC__B_A_
    ABC___BE__ ABC___BE__
    ABC____E_B ABC____E_B
    ABC____E_A ABC____EA_
    ABC____FB_ ABC__FB__
    ABC_____BA ABC___B_A_
SIZE = 6
    ABC_B_BE___ ABCB__BE__
```

```
ABC_B__E_B_ ABC B__E_B
ABC_B__E_A ABC B__EA_
ABC_B___FB_ ABC B_FB___
ABC_B___BA ABC B__B_A_
ABC_B__E_B_ ABC___BE_B
ABC_B__E_A ABC___BEA_
ABC___BE_B_ ABC___BE_B
ABC___BE_A ABC___BEA_
ABC____EFB_ ABC_EFB___
ABC____FBA ABC__FB_A_
SIZE = 7
    ABC_B_BE_B_ ABC B__BE_B
    ABC_B_BE__A ABC B__BEA_
    ABC_B__EFB_ ABC BEFB___
    ABC_B___FBA ABC B_FB_A_
    ABC____EFBA ABC_EFB_A_
SIZE = 8
    ABC_B__EFBA ABC BEFB_A_
```

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00242 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00003 SECONDS

6. 当然，暴力方法所花费的时间会受到打印子序列的额外步骤的影响。通过在将 DEBUG 常量设置为 0 后再次运行我们的代码，输出现在如下：

```
Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00055 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00002 SECONDS
```

7. 当我们完成时，最长公共子序列的长度将包含在 DP[A.size()][B.size()] 中——即当 A 和 B 的前缀都等于整个字符串时 LCS 的值。因此，我们完整的 DP 逻辑写成如下：

```
string LCS_Tabulation(string A, string B)
{
    vector<vector<int>> DP(A.size() + 1, vector<int>(B.size() + 1));

    for(int i = 0; i <= A.size(); i++)
    {
        for(int j = 0; j <= B.size(); j++)
        {
            if(i == 0 || j == 0)
            {
                DP[i][j] = 0;
            }
            else if(A[i-1] == B[j-1])
            {
                DP[i][j] = DP[i-1][j-1] + 1;
            }
            else
            {
                DP[i][j] = max(DP[i-1][j], DP[i][j-1]);
            }
        }
    }

    int length = DP[A.size()][B.size()];
    ....
}
```

到目前为止，我们已经讨论了几种寻找最长公共子序列长度的方法，但如果我们还想输出其实际字符怎么办？当然，我们的暴力解决方案会做到这一点，但效率非常低；然而，使用前一个 DP 表中包含的结果，我们可以使用回溯轻松地重建 LCS。让我们突出显示我们在表中需要遵循的路径来实现这一点：

在 C++ 中，这可以编码如下：

```
string ReconstructLCS(vector<vector<int>> &DP, string &A, string &B, int i, int j)
{
    if(i == 0 || j == 0)
    {
        return "";
    }

    if(A[i-1] == B[j-1])
    {
        return ReconstructLCS(DP, A, B, i-1, j-1) + A[i-1];
    }
    else if(DP[i-1][j] > DP[i][j-1])
    {
        return ReconstructLCS(DP, A, B, i-1, j);
    }
    else
    {
        return ReconstructLCS(DP, A, B, i, j-1);
    }
}
```

9. 现在，我们可以在 LCS_Tabulation() 的最后一行返回 ReconstructLCS() 的结果：

```
string LCS_Tabulation(string A, string B)
{
    ....

    string lcs = ReconstructLCS(DP, A, B, A.size(), B.size());

    return lcs;
}
```

10. 我们 main() 中的代码现在应修改为适应 LCS_Tabulation() 的添加：

```
int main()
{
    string A, B;
    cin >> A >> B;
```

```
int tests = 3;

clock_t timer = clock();

for(int i = 0; i < tests; i++)
{
    int LCS;

    switch(i)
    {
        ....

        case 2:
        {
            string lcs = LCS_Tabulation(A, B);

            LCS = lcs.size();

            cout << "The longest common subsequence of " << A << " and " << B << " is: " << lcs << endl;

            break;
        }
    }
    cout << "Length of the longest common subsequence of " << A << " and " << B << " is: " << LCS << endl;

    GetTime(timer, types[i]);
}
return 0;
}
```

11. 使用字符串 ABCABDBEFBA 和 ABCBEFBEAB，你的程序输出应类似于此：

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING BRUTE FORCE: 0.00060 SECONDS

Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING MEMOIZATION: 0.00005 SECONDS

The longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is: ABCBEFBA
Length of the longest common subsequence of ABCABDBEFBA and ABCBEFBEAB is:
8
TIME TAKEN USING TABULATION: 0.00009 SECONDS

## Note

实际花费的时间值会根据你的系统而有所不同。请注意这些值之间的差异。

现在，我们已经查看了另一个详细的示例，说明如何使用不同技术将相同的逻辑应用于相同的问题，以及这对算法执行时间的影响。

## Activity 21: Melodic Permutations

我们首先要问自己的问题是：这个问题中一个单一的状态构成什么？

Base case --> Empty set:

- 1. 考虑旋律中的每个音符。
- 2. 对于之前遇到过的每个音符子集，要么追加当前音符，要么什么都不做。
- 3. 如果子集与目标匹配，则将其添加到解决方案中。

鉴于我们的选项是追加音符到之前子集或保持原样，我们可以重述逻辑如下：

对于旋律中的给定音符，包含该音符的大小为 | n | 的子集的计数等于不包含该音符的大小为 | n - 1 | 的所有子集的总计数。

因此，每个状态可以用两个维度表示：

- **Dimension 1**: 已考虑的旋律长度。
- **Dimension 2**: 通过取之前找到的子集并要么将其追加到旋律中索引 [length - 1] 处的音符，要么什么都不做而形成的 resulting 子集。

