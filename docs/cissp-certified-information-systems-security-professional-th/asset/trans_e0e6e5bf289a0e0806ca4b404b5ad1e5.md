

美国密码学家克劳德·香农在 1949 年证明，如果密钥是真正随机的、与明文长度相同，并且仅使用一次，则该系统是安全且不可破解的（假设密钥保持秘密）。这种类型的加密被称为 `one-time pad`，因为密钥仅用于一次加密和解密。这种方法的缺点是密钥必须与消息长度相同且不能重复使用，使得该方法除最短的消息或在成本上可以证明安全交付大量密钥材料是合理的的情况（通常是政府）外完全不切实际。然而，量子密码学的发展（参见“量子密码学”部分）将提供安全交付大量随机数据的方法，可用于一次性密码本加密。

实用密码使用固定长度的密钥来加密长度可变的多个消息。正如我们在密码分析部分将看到的那样，如果攻击者获得用同一密钥加密的足够多的密文，一些密码会被破解。定期更换密钥，使每个唯一密钥产生的密文量受限，可以提高密码的安全性。

流密码分为两种类型：同步和自同步。

同步密码要求发送方和接收方保持完美同步才能解密流。如果从流中添加或删除字符（位），解密将从该点开始失败。接收方需要能够检测到同步丢失，并尝试各种偏移以重新同步，或等待发送方插入的独特标记以使接收方能够重新同步。

自同步流密码顾名思义，具有在最多 `N` 个字符后（`N` 是特定自同步流密码的属性），接收方将自动从密文流中删除或添加的字符中恢复的特性。虽然在数据可能被添加到密文流或从中删除的情况下，它们可能是一个明显的优势，但自同步密码的问题在于，如果一个字符被损坏，错误会传播，影响多达下一个 `N` 个字符。对于同步密码，密文中的单个字符错误只会导致解密明文中的单个字符错误。

### Block Ciphers

分组密码一次处理 `n` 位的块，使用大小为 `k` 的密钥。处理块的输出然后成为密码函数的下一次迭代的输入。发生的迭代次数被称为 `rounds`。 [Table 3.4](#c03.xhtml#c03-tbl-0004) 提供了分组密码的概述。

**[TABLE 3.4](#c03.xhtml#R_c03-tbl-0004) Overview of Block Ciphers**

| BLOCK CIPHER || BLOCK SIZE (`N`) || KEY SIZE (`K`) || ROUNDS |

| --- | --- | --- | --- |

| DES || 64 || 56 || 16 |

| --- | --- | --- | --- |

| AES-128 || 128 || 128 || 10 |

| AES-192 || 128 || 192 || 12 |

| AES-256 || 128 || 256 || 14 |

`DES` 密码由于其密钥长度短而不再被认为安全，但它开启了现代密码学的时代，因此在历史上很重要。因为 `DES` 不再安全，找到其替代品至关重要。 `AES` 在从十多个候选中评估和选择算法的五年过程中被选中。 三重 `DES` (`3DES`) 是 `DES` 的重要演进，并在本节后面讨论。

大多数分组密码，包括 `DES` 和 `AES`，都是由多轮数学函数构建的，如 [Figure 3.17](#c03.xhtml#c03-fig-0017) 所示。虽然轮数越多安全性越高，但也会减慢算法过程，因此选择是在安全性和速度之间权衡。

![FIGURE 3.17 Multiple rounds of mathematical functions in block ciphers](images/c03f017.png)

**[FIGURE 3.17](#c03.xhtml#R_c03-fig-0017) Multiple rounds of mathematical functions in block ciphers**

不同分组密码之间的区别在于每个轮次执行的变换，以及加密密钥如何被拉伸然后分割以提供每个轮次的唯一密钥。

构造分组密码使用的一些标准构建块有：

* Substitution or S-boxes ([Figure 3.18](#c03.xhtml#c03-fig-0018))
* Permutations or P-boxes ([Figure 3.19](#c03.xhtml#c03-fig-0019))

![FIGURE 3.18 Block cipher with substitution of S-boxes](images/c03f018.png)

**[FIGURE 3.18](#c03.xhtml#R_c03-fig-0018) Block cipher with substitution of S-boxes**

![FIGURE 3.19 Block cipher with permutation of P-boxes](images/c03f019.png)

**[FIGURE 3.19](#c03.xhtml#R_c03-fig-0019) Block cipher with permutation of P-boxes**

`AES` 使用多轮替换和置换，而 `DES` 使用 16 轮 Feistel 网络。

### Block Cipher Modes of Operation

像 `AES` 这样的分组密码接收八字节的明文并产生八字节的密文。但如果你的消息短于八字节，长于八字节，或不是八字节的倍数怎么办？

为了处理不是密码块长度倍数的消息，一个机制是在加密前添加填充，并在加密后移除填充。有很多方法可以做到这一点，但一种方法是向消息末尾添加字节，每个字节包含已添加字节数的计数（参见 [Figure 3.20](#c03.xhtml#c03-fig-0020)）。由于解密过程将检查最后一个块的最后一个字节以确定添加了多少填充字节（从而需要移除），如果明文是块大小的倍数，则必须添加一个仅包含填充的最终块。

![FIGURE 3.20 Adding padding at the end of a message in a block cipher](images/c03f020.png)

**[FIGURE 3.20](#c03.xhtml#R_c03-fig-0020) Adding padding at the end of a message in a block cipher**

填充并非没有风险，例如稍后在“侧信道攻击”部分描述的填充 Oracle 攻击。

一旦消息被填充为密码块大小的精确倍数，它就可以被加密。最简单、最明显且最不安全的方法（对于较长消息）是 `ECB` 操作模式。在此模式下，明文的每个块由密码独立处理。虽然这对于不超过块大小的消息可能足够，但对于较长消息它有严重的弱点，因为相同的明文块会产生相同的密文块（[Figure 3.21](#c03.xhtml#c03-fig-0021)）。

即使在要加密的数据与块大小相同或更小的情况下（例如数据库中的数字字段），使用 `ECB` 也是不明智的，如果揭示表中不同行具有相同的值可能会损害保密性。作为一个简单的例子，如果使用 `ECB` 加密出生日期字段，那么就可以很容易地确定数据库中出生在同一天的所有人，如果你能确定其中一个人的出生日期，你就会知道所有人的出生日期（具有相同的加密出生日期）。

![FIGURE 3.21 ECB padding produces serious weaknesses for longer messages](images/c03f021.png)

**[FIGURE 3.21](#c03.xhtml#R_c03-fig-0021) ECB padding produces serious weaknesses for longer messages**

`ECB` 的优点，除了其简单性之外，是加密可以并行进行（即在多个处理器之间划分），解密也可以。因此，一个块中的错误不会影响后续块。

在 `CBC` 中，第一个数据块与称为 `IV` 的随机数据块进行 XOR。每个后续的明文块在加密前与前一个密文块进行 XOR。（参见 [Figure 3.22](#c03.xhtml#c03-fig-0022)。）

