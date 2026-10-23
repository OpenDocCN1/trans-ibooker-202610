

## 结论

本章涵盖了最重要的且实用的密码分析以及密码分析指南和方法。你现在可以用以测试和验证密码算法和方法的实现为目标的标准来管理标准。作为总结，你学习了关于

- 密码分析攻击分类
- 密码分析操作
- 标准 FIPS 140-2 和 FIPS 140-3
- 标准 15408
- 密码系统的验证

## 参考文献

[1] Adrian Atanasiu, *Matematici in criptografie*. US Publishing House, Editor: Universul Stiintific, ISBN: 978-973-1944-48-7. 2015.

[2] Adrian Atanasiu, *Securitatea Informatiei vol 1 (Criptografie)*. InfoData Publishing House, ISBN: 978-973-1803-18-0. 2007.

[3] Adrian Atanasiu, *Securitatea Informatiei vol 2 (Protocoale de securitate)*. InfoData Publishing House, ISBN: 978-973-1803-18-0. 2009.

[4] S.J. Knapskog, “Formal specification and verification of secure communication protocols,” 58–73.

[5] K. Koyama, “Direct demonstration of the power to break public-key cryptosystems,” 14–21.

[6] P.J. Lee, “Secure user access control for public networks,” 46–57.

[7] R. Lidl and W.B. Muller, “A note on strong Fibonacci pseudoprimes,” 311–317.

[8] Menezes, S. Vanstone, “The implementation of elliptic curve cryptosystems,” 2–13.

[9] M.J. Mihaljević and J.D. Golić, “A fast iterative algorithm for a shift register initial state reconstruction given the noisy output sequence,” 165–175.

## 第19章

## 线性与差分密码分析

在本章中，我们将讨论两种重要的密码分析类型：*线性* 和 *差分密码分析*。为了说明如何将理论概念与实践相结合，在开始时我们将介绍一套基本概念和高级技术，说明专业人士如何实现这两种类型的密码分析。

尽管某些差分和线性机制已经过时，但仍有大量空间可以找到新的挑战，以便获得新的结果。关于线性与差分密码分析的研究文献提供了大量的理论方法和机制，但只有少数理论能够在实践中应用，以开发针对差分和线性密码分析攻击的专业解决方案。

理论密码分析与应用密码分析之间的差异非常巨大。过去12年中发表的结果，例如算法、方法、博弈论方面等，使研究人员和专业人士走向了不同的道路。其中大多数是奇想的梦幻生物（没有实际应用性的复杂数学系统）和花哨的算法；其他一些在实践中取得了成功的应用。

在密码分析领域开展研究并提高其在实践中和不同场景下的应用潜力需要时间、经验，以及理论家与实践者之间持续的跨协作，而不存在这两种类别之间的隔离。它们在密码分析领域的重要性至关重要，提供必要的工具和机制来构建针对分组密码和流密码的密码分析攻击方案。

### 差分密码分析

差分密码分析由 E. Biham 和 A. Shamir 在90年代初期实现。差分密码分析的目的是检查密文是否以大于其他概率的概率追踪密钥的某些位置。检查过程可以用任何1级顺序进行。实际上，该测试代表了2阶测试周期的复杂近似。

通过差分密码分析，我们将暴露密码算法的弱点。以下差分密码分析示例说明了流密码算法。在下面的伪代码中，我们将按如下方式说明算法：

```
INPUT:  The key is chosen as K = (k₁, ..., kₙ) with kᵢ ∈ {0,1}
```

```
OUTPUT: The weak points of the cryptography
algorithm together with the resistance decision for
differential cryptanalysis
```

- 1. α←拒绝率值
- 2. 选择 n 套具有扰动属性的密钥，从密钥 K 开始。

```
for i = 1 to n do K⁽ⁱ⁾ = (δ₁ᵢ ⊕ k₁, ..., δₙᵢ ⊕ kₙ):
```

```
δₗᵢ = { 1, if j ≠ i, 0, if j = i. }
```

for i,j=1,...,n. 在上述形式中，第 i 个密钥是通过改变第 i 个位置的位从基本密钥获得的。

- 3. 构造密文：第一步是使用基本密钥、扰动密钥和明文 M 构建 n + 1 个密文。我们将得到的密文表示为 C⁽ⁱ⁾，i = 1, ..., n + 1。作为明文 M，我们可以选择文本 0 - 到处都是。

- 4. 构造相关矩阵：这里我们为相关值 C 构建 (n + 1) × (n + 1) 矩阵：

```
cᵢⱼ = correlation(cryptogram i, cryptogram j).
```

相关 $c_{ij}$ 表示应用于序列 ($cryptogram i \oplus cryptogram j$) 的统计测试的值。矩阵 $C$ 表示为对称矩阵，主对角线上有 1。

5. **显著值的计算过程：** 它计算位于主对角线以上的显著相关值的数量。一个值被称为 *显著* 如果

$c_{i,j} \notin \left[ u_{\frac{\alpha}{2}} ; u_{1-\frac{\alpha}{2}} \right]$.

设 $T$ 为显著值的数量，表示相关测试的拒绝次数。

6. **决策和结果解释：** 如果

$\frac{T - \alpha \cdot \frac{n(n+1)}{2}}{\sqrt{\alpha(1-\alpha) \cdot \frac{n(n+1)}{2}}} \notin \left[ u_{\frac{\alpha}{2}} ; u_{1-\frac{\alpha}{2}} \right]$,

计算后，我们可以决定对差分密码分析的非抗性（$u_{\frac{\alpha}{2}}$ 和 $u_{1-\frac{\alpha}{2}}$）表示正态分布的 $\frac{\alpha}{2}$ 和 $1-\frac{\alpha}{2}$ 阶的分位数，并固定 $n \ge i > j \ge 1$ 的 $(i,j)$ 元素，其中 $c_{ij}$ 显著。这些元素代表算法的弱点。否则我们就无法谈论对这种类型攻击的抗性。

列表 19-1 显示了上述伪代码在 C++ 中的实现，图 19-1 所示的输出是自解释的且相当直观。源代码分为七个步骤：

- 生成差分结构作为矩阵
- 计算差异
- 计算使用差分生成的中间值
- 生成已知密钥对
- 计算并寻找一对好的密钥

```
int known_ciphertext_1[10000];
int good_plaintext_0, good_plaintext_1, good_ciphertext_0, good_ciphertext_1;
int pairs_of_numbers;
int characters_dataSet_0[16];
int characters_data_max = 0;
int round_function(int integer_input, int cryptoKey)
{
    return theSBox[cryptoKey ^ integer_input];
}

int encryption(int integer_input, int key_0, int key_1)
{
    int x_value_0 = round_function(integer_input, key_0);
    return x_value_0 ^ key_1;
}

void find_differences()
{
    printf("\nXOR Differential Structure:\n");

    int a, b, c, f;    //c, d, e, f

    for(a = 0; a < 16; a++)
    {
        for(b = 0; b < 16; b++)
        {
            characters[a ^ b][theSBox[a] ^ theSBox[b]]++;
        }
    }

    for(a = 0; a < 16; a++)
    {
        for(b = 0; b < 16; b++)
            printf(" %x ", characters[a][b]);
        printf("\n");
    }
}
```

```
```c
printf("\nShowing the most important differences:\n");

for(a = 0; a < 16; a++)
{
    for(b = 0; b < 16; b++)
    {
        if (characters[a][b] == 6)
        {
            printf("  6/16:  %i {--} %i\n", a, b);
        }
    }
}

void generate_characters_data(int input_differences,
                              int output_differences)
{
    printf("\nIntermediate values generated using the\ndifferentials(%i --> %i):\n", input_differences,
           output_differences);

    characters_data_max = 0;
    int f;
    for(f = 0; f < 16; f++)
    {
        int computations = f ^ input_differences;

        if ((theSBox[f] ^ theSBox[computations]) ==
            output_differences)
        {
            printf("  The possibility:  %i + %i {--}\n"
                   "                     %i + %i\n", f, computations,
                   theSBox[f], theSBox[computations]);
            characters_dataSet_o[characters_data_max] = f;
            characters_data_max++;
        }
    }
}
```
```
