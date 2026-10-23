

## 使用单字母、双字母和三字母频率计数进行密码分析

频率分析是寻找密文中字符出现次数的最佳实践之一，其目标是破解密码。基于模式分析的分析可用于测量和计数文本中出现的字符对，即双字母（或双图），该方法用于测量文本中出现的字符对。三字母频率分析测量由三个字母组成的组合的出现次数。

在本节中，我们将重点介绍使用双字母和三字母对文本进行表征的方法，这些方法可用于解决诸如 Playfair 之类的密码。

## 计数单字母

计数单字母是替代密码（如凯撒密码、波利比乌斯方阵等）中使用最有效的方法之一。该方法之所以非常有效，是因为英语有特定的频率分布。这也意味着它不会被替代密码所隐藏。该分布如图 22-4 所示。

![](img/c3d9424f64735276d661e7aba725b2a2_440_0.png)

图 22-4. 英语字母频率

## 计数双字母

双字母计数基于与单字母计数相同的思想。不同之处在于，你不是统计单个字符的出现次数，而是统计字符对的出现频率。

图 22-5 列出了密码分析过程中遇到的一些常见双字母。在清单 22-2 中，我们实现了一个统计双字母出现次数的解决方案。图 22-6 显示了此双字母计数示例的输出。清单 22-2 中的源代码使用名为 bigram.txt 的文件，其中包含一个示例文本以说明双字母计数过程。

图 22-5. 双字母

```
TH 11699784
HE 10068926
IN 87674002
ER 77134382
AN 69775179
RE 60923600
ES 57070453
ON 56915252
ST 54018399
NT 50701084
EN 48991276
AT 48274564
ED 46647960
ND 46194306
TO 46115188
OR 45725191
EA 43329810
TI 42888666
AR 42353262
TE 42295813
NG 38567365
AL 38211584
IT 37938534
AS 37773878
IS 37349981
HA 35971841
ET 32872552
SE 31532272
OU 31112284
OF 30540904
```

图 22-6. 计数双字母

## 清单 22-2. 计数双字母

```
#include <stdio.h>

int main(void)
{
    int alphabet_counting['z' - 'a' + 1]['z' - 'a' + 1] = {{ 0 }};
    int character0 = EOF, character1;
    FILE *fileBigramSampleText = fopen("bigram.txt", "r");

    if (fileBigramSampleText != NULL)
    {
        while (((character1 = getc(fileBigramSampleText)) != EOF))
        {
            if (character1 >= 'a' && character1 <= 'z' && character0 >= 'a'
            && character0 <= 'z')
            {
                alphabet_counting[character0 - 'a'][character1 - 'a']++;
            }
            character0 = character1;
        }
        fclose(fileBigramSampleText);
        for (character0 = 'a'; character0 <= 'z'; character0++)
        {
            for (character1 = 'a'; character1 <= 'z'; character1++)
```

```
char *position0 = NULL;
int character1;
FILE *fileTextForCountingBigrams = fopen("bigram.txt", "r");

memset(count, 0, sizeof(count));

if (fileTextForCountingBigrams != NULL)
{
    while ((character1 = getc(fileTextForCountingBigrams)) != EOF)
    {
        char *p1 = (char*)memchr(alphabet, character1, length_of_alphabet);
        if (p1 != NULL && position0 != NULL)
        {
            count[position0 - alphabet][p1 - alphabet]++;
        }
        position0 = p1;
    }
    fclose(fileTextForCountingBigrams);
    for (size_t i = 0; i < length_of_alphabet; i++)
    {
        for (size_t j = 0; j < length_of_alphabet; j++)
        {
            int n = count[i][j];
            if (n > 0)
            {
                printf("%c%c: %d\n", alphabet[i], alphabet[j], n);
            }
        }
    }
}
return 0;
}
```

## 计数三字母

三字母计数遵循与双字母计数相同的原理；不同之处在于计数三个字符。

## 结论

本章介绍了文本表征的概念，并展示了其在密码分析过程中的重要性。你现在可以处理卡方统计量，并可以使用单字母、双字母和三字母来解密替代密文。总结来说，你学习了关于

- 文本表征的概念
- 使用单字母、双字母和三字母
- 实现卡方统计量
- 单字母、双字母和三字母的实现

## 参考文献

[1] Simon Singh, *The Code Book: The Science of Secrecy from Ancient Egypt to Quantum Cryptography*. ISBN 0-385-49532-3. 2000

[2] Helen F. Gaines, *Cryptanalysis: A Study of Ciphers and Their Solution*. 1989.

## 第 23 章

## 密码分析方法的实现与实践方法

本章是对密码分析方法的方法论的一般讨论，以及这些方法如何以快速高效的方式应用。所提出的方法论是经典和现代的密码学/密码分析算法和方法。量子密码学目前不包含在内。

所提出的方法论（见图 23-1）旨在帮助你，密码分析员，在密码分析过程中了解自己的位置。你可以使用图 23-1 中呈现的地图来为你的工作选择合适的工具或方法。

如果没有关于加密方法的正确信息，实现密码分析方法可能是一项非常繁琐的任务。以下将介绍识别进行密码分析过程所需元素的简短流程。密码分析过程包括两个总体步骤：

- *步骤 1* 基于确定应进行何种类型的密码分析。
- *步骤 2* 包括收集你所知道的关于密码算法的一切。在正确且尽可能认真地执行了这两个步骤之后，你可以继续进行额外步骤。
- *步骤 3* 是你构建适当的 *攻击模型* 时。
- *步骤 4* 是你选择适当的工具时。

*步骤 1.* 此步骤涉及应执行何种密码分析。在此步骤中，密码分析员将在业务环境中决定其将扮演的角色：合法且授权的密码分析员、道德黑客或恶意破解者。一旦你决定了自己的角色，就可以进入步骤 2。

*步骤 2.* 如果密码分析员是合法的，他们在开始之前必须意识到两件事：*密码算法* 和 *加密密钥*。根据一些密码分析员的经验，这不是必要要求，但在某些情况下知道这些信息将非常有用。一旦你了解了密码算法和加密密钥，你就可以轻松开始密码分析过程，通过应用适当的方法并测试业务应用程序的安全性。

*步骤 3.* 此步骤基于设置攻击模型或攻击类型。攻击模型和攻击类型将指出一个定量变量，用于表示密码分析员在对加密消息执行破解方法时将能够访问多少信息。最重要的攻击是

- 仅密文攻击
- 已知明文攻击
- 选择明文攻击
- 选择密文攻击
  - 自适应选择密文攻击
  - 不分选择的密文攻击

步骤 4. 在选择了攻击模型或创建并根据案例和要求正确调整了其他模型之后，你进入下一步，即选择软件工具。从已存在的工具中选择软件工具或创建自己的工具可能耗时，但实践中将产生巨大贡献。总结如下，这里有一些可在密码分析过程中使用的工具，具体取决于你试图测试的内容。

- **渗透工具：** Kali Linux, Parrot Security, BackBox
- **取证：** DEFT, CAINE, BlackArch, Matriux
- **数据库：** sqlmap（独立版本）, Metasploit framework（独立版本）, VulDB

