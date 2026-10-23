

// Driver program to test the above function
int main()
{
    string text="THEQUICKBROWNFOXJUMPSOVERTHELAZYDOG";
    int s = 4;
    cout << "Text : " << text;
    cout << "\nShift: " << s;
    cout << "\nCipher: " << encrypt(text, s);
    return 0;
}
```

## C++20 中的 Vigenère 密码实现

Vigenère 密码（见图 2-10 和清单 2-3）是使用基于关键词密钥的不同凯撒密码序列对字母文本进行加密的经典方法之一。你可以在某些文档中看到它被归为一种多字母替换类型。

![](img/c3d9424f64735276d661e7aba725b2a2_68_0.png)

图 2-10. Vigenère 密码

该密码的简短代数描述如下。数字被视为数字（A = 0，B = 1，等等）并执行模 26 的加法运算。使用 K 作为密钥的 Vigenère 加密 E 可以写成

C_i = E_K (M_i) = (M_i + K_i) mod 26

而使用密钥 K 的解密 D 为

M_i = D_K (C_i) = (C_i - K_i) mod 26
其中 *M* = *M*₁...*M*ₙ 是消息，*C* = *C*₁...*C*ₙ 表示密文，*K* = *K*₁...*K*ₙ 表示通过重复关键词 ⌈*n*/*m*⌉ 次得到的密钥，其中 *m* 表示关键词长度。

### 清单 2-3. Vigenère 密码的源代码

```cpp
#include <iostream>
#include <string>
using namespace std;
class Vigenere {
    public:
        /** represents the key */
        string key;

        /** the constructor of the class */
        /** the chosen key */
        Vigenere(string chosenKey) {
        for (int i = 0; i < chosenKey.size(); ++i) {
            if (chosenKey[i] >= 'A' && chosenKey[i] <= 'Z')
                this->key += chosenKey[i];
            else if (chosenKey[i] >= 'a' && chosenKey[i] <= 'z')
                this->key += chosenKey[i] + 'A' - 'a';
        }
    }
    string encrypt(string t)
    {
        string encryptedOutput;
        for (int i = 0, j = 0; i < t.length(); ++i) {
            char c = t[i];
            if (c >= 'a' && c <= 'z')
                c += 'A' - 'a';
            else if (c < 'A' || c > 'Z')
                continue;
            /** added 'A' to bring it in range */
            /** of ASCII alphabet [ 65-90 | A-Z ] */
```

## 结论

在本章中，我们对密码学原语和机制的基础进行了简短介绍。本章涵盖了以下内容：

- 安全和信息安全目标
- 在设计和实现密码学函数中，一对一、单向和陷门单向函数的重要性
- 数字签名及其工作原理
- 公钥密码学及其对开发应用程序的影响
- 哈希函数
- 案例研究，用于说明在进入高级密码学概念之前需要了解的基本概念

下一章将介绍概率论、信息论、数论和有限域的基础知识。我们将讨论它们的重要性以及它们与已存在于 C++ 中的实现之间的关系，以及它们对你作为开发人员的有用性。

## 参考文献

[1] Simon Singh, *The Code Book: The Secrets Behind Codebreaking*, 2003.

[2] W. Diffie and M. Hellman, “New directions in cryptography” in *IEEE Trans. Information Theory*. 22, 6 (September 2006), 644–654. 2006. DOI: https://doi.org/10.1109/TIT.1976.1055638.

[3] R. L. Rivest, A. Shamir, and L. Adleman, “A method for obtaining digital signatures and public-key cryptosystems,” in *Communications ACM*, vol. 21, no. 2, pp. 120–126, 1978.

[4] T. ElGamal, “A Public Key Cryptosystem and a Signature Scheme Based on Discrete Logarithms” in G.R. Blakley and D. Chaum (eds) *Advances in Cryptology. CRYPTO 1984. Lecture Notes in Computer Science, vol 196*. Springer, Berlin, Heidelberg.

[5] ISO/IEC 9796-2:2010 – Information Technology – Security Techniques – Digital Signature schemes giving message recovery. Available: https://www.iso.org/standard/54788.html.

[6] Bruce Schneier and Phil Sutherland, *Applied Cryptography: Protocols, Algorithms, and Source Code in C*, second edition. ISBN: 978-0-471-12845-8. USA: John Wiley & Sons, Inc. 1995.

[7] William Stallings, *Cryptography and Network Security: Principles and Practice*. Upper Saddle River, N.J: Prentice Hall, 1999. Print.

[8] Douglas R. Stinson, *Cryptography: Theory and Practice*, first edition. ISBN: 978-0-8493-8521-6, CRC Press, Inc., USA. 1995.

[9] Neal Koblitz, *A Course in Number Theory and Cryptography*. New York: Springer-Verlag, 1994. Print.

[10] Neal Koblitz and A J. Menezes, *Algebraic Aspects of Cryptography*, 1999. Print.

[11] Oded Goldreich, *Foundations of Cryptography: Basic Tools*. Cambridge: Cambridge University Press, 2001. Print.

[12] Oded Goldreich, *Modern Cryptography, Probabilistic Proofs and Pseudorandomness*. Berlin: Springer, 1999. Print.

[13] Michael G. Luby, *Pseudorandomness and Cryptographic Applications*. Princeton, NJ: Princeton University Press, 1996. Print.

[14] Bruce Schneier, *Secrets and Lies: Digital Security in a Networked World*. New York: John Wiley, 2000.

[15] Peter Thorsteinson and Arun Ganesh, *.NET Security and Cryptography*. Prentice Hall Professional Technical Reference, 2003.

[16] Adrian Atanasiu, *Criptografie (Cryptography) – Volume 1*. Publisher House: InfoData. ISBN: 978-973-1803-29-6, 978-973-1803-16-6. 2007. Available in Romanian Language.

[17] Adrian Atanasiu, *Protocoale de Securitate (Security Protocols)* - Volume 2. Publisher House: InfoData. ISBN: 978-973-1803-29-6, 978-973-1803-16-6. 2007. Available in Romanian Language.

[18] Alfred J. Menezes, Scott A. Vanstone, and Paul C. Van Oorschot, *Handbook of Applied Cryptography* first edition. CRC Press, Inc., USA, ISBN: 978-0-8493-8523-0. 1996.

[19] Namespace System.Security.Cryptography. Available: https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography?view=netframework-4.8.

[20] Henri Cohen, Gerhard Frey, Roberto Avanzi, Christophe Doche, Tanja Lange, Kim Nguyen, and Frederik Vercauteren, *Handbook of Elliptic and Hyperelliptic Curve Cryptography*, second edition. Chapman & Hall/CRC. 2012.

[21] OpenPGP Library for .NET. Available online: www.didisoft.com/net-openpgp/

[22] Bouncy Castle .NET. Available online: www.bouncycastle.org/csharp/.

[23] Nethereum. Available from: https://github.com/Nethereum.

[24] Botan. Available online: https://botan.randombit.net/.

[25] Cryptlib. Available online: www.cs.auckland.ac.nz/~pgut001/cryptlib/.

[26] Crypto++. Available online: www.cryptopp.com/.

[27] Libgcrypt. Available online: https://gnupg.org/software/libgcrypt/.

[28] Libsodium. Available online: https://nacl.cr.yp.to/.

[29] Nettle. Available: www.lysator.liu.se/~nisse/nettle/.

[30] OpenSSL. Available: www.openssl.org/.

[31] J. Guo, P. Karpman, I. Nikolić, L. Wang, and S. Wu, “Analysis of BLAKE2. In: Benaloh J. (eds) Topics in Cryptology – CT-RSA 2014” in Lecture Notes in Computer Science, vol 8366. Springer, Cham. 2014.

[32] Blake3. Available online: https://github.com/BLAKE3-team/BLAKE3/.

[33] H. Krawczyk, M. Bellare, and R. Canetti, “HMAC: keyed – Hashing for Message Authentication.” RFC 2104, 1997.

[34] API KMAC. Available online: www.cryptosys.net/manapi/api_kmac.html.

[35] John Kelsey, Shu-jen Chang, Ray Perlner, “SHA-3 Derived Functions: cSHAKE, KMAC, TupleHash and ParallelHash” in NIST Special Publication 800-185. National Institute of Standards and Technology, December 2016.

[36] I.B. Damgard, “A design principle for hash functions” in LNCS 435 (1990), pp. 516-527.

[37] Ronald L. Rivest, “The MD6 hash function. A proposal to NIST for SHA-3.” Available online: http://groups.csail.mit.edu/cis/md6/submitted-2008-10-27/Supporting_Documentation/md6_report.pdf.

[38] PMAC. Available online: https://web.cs.ucdavis.edu/~rogaway/ocb/pmac.htm.

[39] UMAC. Available online: http://fastcrypto.org/umac/.

[40] BLAKE-256. Available online: https://docs.decred.org/research/blake-256-hash-function/.

[41] Eli Biham and Orr Dunkelman, “A Framework for Iterative Hash Functions - HAIFA. Second NIST Cryptographic Hash Workshop” via Cryptology ePrint Archive: Report 2007/278. August 24, 2006.

[42] BLAKE2 Official Implementation. Available online: https://github.com/BLAKE2/BLAKE2.

[43] GOST. Available online: https://tools.ietf.org/html/rfc5830.
