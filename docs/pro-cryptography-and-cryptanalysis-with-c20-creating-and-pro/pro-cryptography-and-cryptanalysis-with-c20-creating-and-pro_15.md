# CHAPTER 20

# Integral Cryptanalysis

积分密码分析是一种针对基于替代-置换网络构造的分组密码设计的密码分析技术。由于积分密码分析攻击可以针对 Square 分组密码 [1] 发起，因此也被称为 Square 攻击。它由 Lars Knudsen 设计。

分组密码的一个暴露点是替代-置换网络。当网络可以被发现时，密码分析对整个密码系统的负面影响就很高。分组密码的另一个暴露点是密钥本身以及密钥置换中涉及的表。当错误密钥与正确密钥相似或相同 时，系统就会被攻破。

在下一节中，我们将介绍可用于实现的关于分组密码的形式基础。进一步，我们将介绍启动积分密码分析攻击所需的元素，例如构建 Feistel 网络和为密码密钥生成置换表。一旦你对这两个阶段有清晰的理解，就很清楚积分密码分析必须如何进行。

# Basic Notions

要实现和设计积分密码分析攻击，在开始实现攻击之前非常重要的是要有形式要素。继续前进，让我们看一下以下概念作为主要起点。它们仅用于教育目的设计和实现此类攻击。

将 $(G, +)$ 视为一个有限阿贝尔群，其阶为 $k$。乘积群 $G^n = G \times ... \times G$ 是一个元素组，其结构为 $v = (v_1, ..., v_n)$，其中 $v_i \in G$。$G^n$ 内的加法定义为逐分量，因此，我们有 $u + v = w$ 对 $u, v, w \in G^n$ 成立，当且仅当对所有 $i$ 有 $u_i + v_i = w_i$。




我们将 B 表示为包含多个向量的集合。我们定义 B 上的积分。该积分表示 B 中所有向量的总和。积分定义为 ∫B = Σ_{ν∈B} ν，其中加法运算由 G^n 上的群运算定义。

在设计积分密码分析时，了解明文和密文中的字数非常重要。在本章的示例中，该数字用 n 表示。另一个需要了解的重要数字是明文和密文的个数，用 m 表示。通常，m = k（即 k = |G|），向量 ν ∈ B 表示明文和密文，且 G = GF(2^b) 或 G = Z/kZ。

进一步深入该攻击，它基于这样一个事实：所涉及的实体之一将在特定轮数的加密之后对积分中的值做出预测。牢记这一点，可以区分三种情况：（1）当字具有相同的值时（例如在 i 处），（2）当字具有不同的值时，以及（3）当提前预测了特定值的总和时。

进一步，考虑如上所述的 B ⊆ G^n 以及一个固定索引 i。可以区分以下三种情况：

- (1) 对于所有 ν ∈ B，有 ν_i = c
- (2) {ν_i : ν ∈ B} = G
- (3) Σ_{ν∈B} ν_i = c'

其中 c, c' ∈ G 是预先已知并固定的某些值。

我们进一步给出的示例是一种常见情况，其中 m = k，即 B 中向量的数量与所考虑群中元素的数量相同。根据拉格朗日定理，如果所有字（置于第 i 个位置的一般字）都具有相同的值，那么直观上，积分中的第 i 个字将具有 G 中单位元的值。

以下两个定理是必要的，对于任何想要将积分密码分析付诸实践的实际开发人员来说，它们都是必备的。

### 定理 20-1 [1, 定理 1, 第 114 页]

考虑 (G, +) 是一个有限阿贝尔加法群。阶为 1 或 2 的元素子群记为 L = {g ∈ G : g + g = 0}。我们将 s(G) 视为 G 中所有元素的和 Σ_{g∈G} g。接下来，我们考虑 s(G) = Σ_{h∈L} h。此外，理解以下类比非常重要：s(G) ∈ L，即 s(G) + s(G) = 0。

根据定理 20-1，对于 G = GF(2^b)，有 s(G) = 0；对于 Z/mZ，若 m 为偶数，则 s(Z/mZ) = m/2，否则为 0。以下定理表示有限阿贝尔群的乘法情形（见定理 20-2）。

### 定理 20-2 [1, 定理 2, 第 114 页]

考虑 (G, *) 是一个有限阿贝尔乘法群。阶为 1 或 2 的元素子群记为 H = {g ∈ G : g * g = 1}。我们将 p(G) 视为 G 中所有元素的乘积 ∏_{g∈G} g。接下来，我们考虑 p(G) = ∏_{h∈H} h。此外，理解以下类比非常重要：p(G) ∈ H，即 p(G) * p(G) = 1。

例如，如果我们有 G = (Z/pZ)*，其中 p 是素数，则 p(G) 将为 −1，即 p(G) = −1。这可以通过威尔逊定理证明。

## 实践方法

在本节中，我们将使用 C++ 编程语言实现一种可在实践中应用的积分密码分析攻击。

以下方法展示了 C++ 中积分密码分析的基本实现，并允许覆盖当前块的大小（通过 revision_sbox_value 覆盖 sboxValue）。在所提供的实现中，我们使用了重复序列。重复序列的目标是制造一个弱点，以展示如何利用该弱点并发起攻击。

在清单 20-1 的源代码中，我们演示了积分密码分析攻击，并提供了有关构建和设计原则的必要注释。在清单 20-1 中，您可以看到积分密码分析攻击的实现方式。一旦您拥有了程序的主内核，为了进一步确保攻击成功，关键在于“弄清楚”攻击是如何设计的，然后创建它的副本或与原始版本相似的东西。

要使用以下示例，您必须从命令提示符运行以下命令（见图 20-1）：

```bash
g++ -std=c++2a integral.cpp -o integral
```

```cpp
//** 良好可靠的加密密钥
int reliable_crypto_keys[16];
//** 猜测加密密钥的值
int guessing_crypto_key1, guessing_crypto_key2;
//** 加密密钥的测试控制变量
int cryptoKey1_testing;

//** 应用掩码
int applying_the_mask(int theValue, int mask_value)
{
    int internal_value = theValue & mask_value;
    int total_value = 0;

    while(internal_value > 0)
    {
        int temporary = internal_value % 2;
        internal_value /= 2;

        if (temporary == 1)
            total_value = total_value ^ 1;
    }
    return total_value;
}

//** 检测近似
void detect_the_approximation()
{
    int a, b, c;

    //** 解析输出掩码。16 表示
    //** s-Box 的大小
    for(a = 1; a < 16; a++)
    {
        //** 解析输入掩码
        for(b = 1; b < 16; b++)
        {
            //** 解析输入
            for(c = 0; c < 16; c++)
            {
                if (applying_the_mask(c, b) ==
                    applying_the_mask(sboxValue[c], a))
                    approximation_array_structure[b][a]++;
            }
        }
    }
}

/** 显示并展示近似 */
void display_the_approximation()
{
    printf("Integral Values Approximations: \n");
    int a, b, c;
    for(a = 1; a < 16; a++)
    {
        for(b = 1; b < 16; b++)
        {
            if (approximation_array_structure[a][b] == 14)
                printf(" %i : %i {<-->} %i\n",
                    approximation_array_structure[a][b], a, b);
        }
    }
    printf("\n");
}

/** s 的轮函数 */
int rounding_procedure(int dataInput, int substitution_key)
{
    return sboxValue[dataInput ^ substitution_key];
}

//** 填充已知的可能密钥值
void filling_up_the_knowings()
{
    int substitution_key_1 = rand() % 16;
    int substitution_key_2 = rand() % 16;

    printf("Generating Data:  Key 1 = %i, Key 2 = %i\n", substitution_key_1, substitution_key_2);

    //** 解析每个已知值，并为每个已知明文
    //** 和密文计算适当的值
    for(counter1 = 0; counter1 < known_numbers; counter1++)
    {
        known_plaintext[counter1] = rand() % 16;
        known_ciphertext[counter1] = rounding_procedure(rounding_procedure(known_plaintext[counter1], substitution_key_1), substitution_key_2);
    }

    printf("Generating Data:  We have generated %i known pairs\n\n", known_numbers);
}

//** 验证并测试加密密钥
void testing_the_keys(int key_1, int key_2)
{
    for(counter2 = 0; counter2 < known_numbers; counter2++)
        if (rounding_procedure(rounding_procedure(known_plaintext[counter2], key_1), key_2) != known_ciphertext[counter2])
            break;
    printf("* ");
}

for(c = 0; c < 16; c++)
{
    int score = cryptographyKey_score_value[c] *
                cryptographyKey_score_value[c];
    if (score > maximum_score) maximum_score = score;
}

for(d = 0; d < 16; d++)
{
    reliable_crypto_keys[d] = -1;
}

d = 0;

for(c = 0; c < 16; c++)
{
    if ((cryptographyKey_score_value[c] *
        cryptographyKey_score_value[c]) == maximum_score)
    {
        reliable_crypto_keys[d] = c;
        printf("Integral Cryptanalysis Attack --> Candidate for K1 = %i\n",
            reliable_crypto_keys[d]);
        d++;
    }
}

for(d = 0; d < 16; d++)
{
    if (reliable_crypto_keys[d] != -1)
    {
        cryptoKey1_testing =
            rounding_procedure(known_plaintext[0],
            reliable_crypto_keys[d]) ^
            revision_sbox_value[known_ciphertext[0]];
```




