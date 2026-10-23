

```cpp
if ((t == 20) & displayT1Computation) {
    cout << "h: 0x" << hex << h << "  dec:" << dec << h
         << "  sign:" << dec << (int)h << endl;
    cout << "EP1(e): 0x" << hex << EP1(e) << "  dec:"
         << dec << EP1(e) << "  sign:" << dec << (int)EP1(e)
         << endl;
    cout << "CH(e,f,g): 0x" << hex << CH(e,f,g) << "  dec:"
         << dec << CH(e,f,g) << "  sign:" << dec
         << (int)CH(e,f,g) << endl;
    cout << "constantOf32BitWords[t]: 0x" << hex
         << constantOf32BitWords[t] << "  dec:" << dec
         << constantOf32BitWords[t] << "  sign:" << dec
         << (int)constantOf32BitWords[t] << endl;
    cout << "Word[t]: 0x" << hex << Word[t]
         << "  dec:" << dec << Word[t] << "  sign:" << dec
         << (int)Word[t] << endl;
    cout << "temporary_1 = 0x" << hex << temporary_1
         << "  dec:" << dec
         << temporary_1 << "  sign:" << dec << (int)temporary_1 << endl;
}

//** according to the NIST Standard and Specification,
//** the BSIG0 is incorrect. We will replace it with EP0
temporary_2 = EP0(a) + MAJ(a, b, c);

//** in order to get T2 we will display the variables
//** and operations
if ((t == 20) & displayT2Computation) {
    cout << "a: 0x" << hex << a << "  dec:" << dec << a
         << "  sign:" << dec << (int)a << endl;
    cout << "b: 0x" << hex << b << "  dec:" << dec << b
         << "  sign:" << dec << (int)b << endl;
    cout << "c: 0x" << hex << c << "  dec:" << dec << c
         << "  sign:" << dec << (int)c << endl;
    cout << "EP0(a): 0x" << hex << EP0(a) << "  dec:"
         << dec << EP0(a) << "  sign:" << dec << (int)EP0(a)
         << endl;
    cout << "MAJ(a,b,c): 0x" << hex
         << MAJ(a, b, c) << "  dec:"
         << dec << MAJ(a, b, c) << "  sign:" << dec
         << (int)MAJ(a, b, c) << endl;
    cout << "temporary_2 = 0x" << hex << temporary_2
         << "  dec:" << dec
         << temporary_2 << "  sign:" << dec << (int)temporary_2 << endl;
}

//** according to the NIST standard
h = g;
g = f;
f = e;

//** Get the guarantee that we are still using 32 bits
e = (d + temporary_1) & 0xFFFFFFFF;
d = c;
c = b;
b = a;

//** Get the guarantee that we are still using 32 bits
a = (temporary_1 + temporary_2) & 0xFFFFFFFF;

//** display the content of each of the variables from
//** above according to the NIST standard.
if (displayWorkVariablesForT) {
    cout << "t= " << t << " ";
    cout << displayAsHex(a) << " " << displayAsHex(b)
         << " " << displayAsHex(c) << " " << displayAsHex(d) << " "
         << displayAsHex(e) << " " << displayAsHex(f) << " "
         << displayAsHex(g) << " " << displayAsHex(h) << " " << endl;
}

//** display the content of each of the hash segment
if (displayTheHashSegments) {
    cout << "InitialHashValueFor32Bit_0 = " << displayAsHex
         (InitialHashValueFor32Bit_0) << " + " << displayAsHex(a) << " " <<
         displayAsHex(InitialHashValueFor32Bit_0 + a) << endl;

    cout << "InitialHashValueFor32Bit_1 = " << displayAsHex
         (InitialHashValueFor32Bit_1) << " + " <<
         displayAsHex(b) << " " << displayAsHex
         (InitialHashValueFor32Bit_1 + b) << endl;

    cout << "InitialHashValueFor32Bit_2 = " << displayAsHex
         (InitialHashValueFor32Bit_2) << " + " <<
         displayAsHex(c) << " " << displayAsHex
         (InitialHashValueFor32Bit_2 + c) << endl;

    cout << "InitialHashValueFor32Bit_3 = " << displayAsHex
         (InitialHashValueFor32Bit_3) << " + " <<
         displayAsHex(d) << " " << displayAsHex
         (InitialHashValueFor32Bit_3 + d) << endl;

    cout << "InitialHashValueFor32Bit_4 = " << displayAsHex
         (InitialHashValueFor32Bit_4) << " + " <<
         displayAsHex(e) << " " << displayAsHex
         (InitialHashValueFor32Bit_4 + e) << endl;

    cout << "InitialHashValueFor32Bit_5 = " << displayAsHex
         (InitialHashValueFor32Bit_5) << " + " <<
         displayAsHex(f) << " " << displayAsHex
         (InitialHashValueFor32Bit_5 + f) << endl;

    cout << "InitialHashValueFor32Bit_6 = " << displayAsHex
         (InitialHashValueFor32Bit_6) << " + " <<
         displayAsHex(g) << " " << displayAsHex
         (InitialHashValueFor32Bit_6 + g) << endl;

    cout << "InitialHashValueFor32Bit_7 = " << displayAsHex
         (InitialHashValueFor32Bit_7) << " + " << displayAsHex
         (h) << " " << displayAsHex(InitialHashValueFor32Bit_7
         + h) << endl;
}

//** for each hash add all the variables in order to be sure that
//** we are still on the page with the 32 bit values
InitialHashValueFor32Bit_0 = (InitialHashValueFor32Bit_0 + a) & 0xFFFFFFFF;
InitialHashValueFor32Bit_1 = (InitialHashValueFor32Bit_1 + b) & 0xFFFFFFFF;
InitialHashValueFor32Bit_2 = (InitialHashValueFor32Bit_2 + c) & 0xFFFFFFFF;
InitialHashValueFor32Bit_3 = (InitialHashValueFor32Bit_3 + d) & 0xFFFFFFFF;
InitialHashValueFor32Bit_4 = (InitialHashValueFor32Bit_4 + e) & 0xFFFFFFFF;
InitialHashValueFor32Bit_5 = (InitialHashValueFor32Bit_5 + f) & 0xFFFFFFFF;
InitialHashValueFor32Bit_6 = (InitialHashValueFor32Bit_6 + g) & 0xFFFFFFFF;
InitialHashValueFor32Bit_7 = (InitialHashValueFor32Bit_7 + h) & 0xFFFFFFFF;

//** add the hash section in one piece one after the other in
//** order to obtain the 256 bit hash
return displayAsHex(InitialHashValueFor32Bit_0) +
       displayAsHex(InitialHashValueFor32Bit_1) +
       displayAsHex(InitialHashValueFor32Bit_2) +
       displayAsHex(InitialHashValueFor32Bit_3) +
       displayAsHex(InitialHashValueFor32Bit_4) +
       displayAsHex(InitialHashValueFor32Bit_5) +
       displayAsHex(InitialHashValueFor32Bit_6) +
       displayAsHex(InitialHashValueFor32Bit_7);
}
```

哈希函数通常用于数字签名和数据完整性。在处理数字签名时，长消息通常会被哈希，而只有哈希值会被签名。接收消息的各方随后会对收到的消息进行哈希，并检查收到的签名是否对该哈希值正确。下面可以看到带密钥的密码学哈希函数的分类（见表 2-2）和不带密钥的密码学哈希函数的分类（见表 2-3）。大多数函数已在 NIST 标准或其他可信资源中以 C++ 实现，例如 CrypTool³。

### 表 2-2. 带密钥的密码学哈希函数

| 名称 | 标签长度 | 类型 | 参考文献 |
|---|---|---|---|
| BLAKE2 | 任意 | 带前缀-MAC 的带密钥哈希函数 | [31][42] |
| BLAKE3 | 任意 | 带提供初始化向量（IV）的带密钥哈希函数 | [32] |
| HMAC | - | - | [33] |
| KMAC | 任意 | 基于 Keccak | [34][35] |
| MD6 | 512 bits | 带有 NLFSR 的 Merkle 树 | [37] |
| PMAC | - | - | [38] |
| UMAC | - | - | [39] |

### 表 2-3. 不带密钥的密码学哈希函数

| 名称 | 长度 | 类型 | 参考文献 |
|---|---|---|---|
| BLAKE-256 | 256 bits | HAIFA 结构 [41] | [40] |
| BLAKE-512 | 512 bits | HAIFA 结构 [41] | [40] |
| GOST | 256 bits | Hash | [43] |
| MD2 | 128 bits | Hash | |
| MD4 | 128 bits | Hash | [44] |
| MD5 | 128 bits | Merkle-Damgård 构造 [36] | [45] |
| MD6 | Up to 512 bits | Merkle-tree NLFSR | [37] |
| RIPEMD | 128 bits | Hash | [46] |
| RIPEMD-128 | 128 bits | Hash | [46][47][48] |
| RIPEMD-256 | - | Hash | |
| RIPEMD-160 | 160 bits | Hash | |
| RIPEMD-320 | 320 bits | Hash | |
| SHA-1 | 160 bits | Merkle-Damgård 构造 [36] | [61] |

## 第 2 章 密码学基础

![](img/c3d9424f64735276d661e7aba725b2a2_67_0.png)

**图 2-9.** *Caesar 密码的执行*

该应用程序非常简单且易于交互。

### 列表 2-2. 凯撒密码实现的源代码

```cpp
#include <iostream>
using namespace std;

// This function receives text and shift and
// returns the encrypted text
string encrypt(string text, int s)
{
    string result = "";

    // traverse text
    for (int i = 0; i < text.length(); i++)
    {
        // apply transformation to each character
        // Encrypt Uppercase letters
        if (isupper(text[i]))
            result += char(int(text[i] + s - 65) % 26 + 65);

        // Encrypt Lowercase letters
        else
            result += char(int(text[i] + s - 97) % 26 + 97);
    }

    // Return the resulting string
    return result;
}
```
