

在下一章中，我们将探讨量子计算和密码学在不久的将来所带来的许多其他令人兴奋的技术。

## 参考文献

- Elliott, C. The DARPA Quantum Network. 2004. Retrieved from arXiv:quant-ph/0412029
- Peev, M., Pacher, C., Alleaume, R. et al. The SECOQC quantum key distribution network in Vienna. New Journal of Physics, Volume 11, 2009.
- D. Stucki, M. Legre, F. Buntschu et al. Long term performance of the SwissQuantum quantum key distribution network in a field environment. New Journal of Physics, 13 123001, 2011.
- Kramer, H. QUESS (Quantum Experiments at Space Scale) / Micius. Retrieved from: https://directory.eoportal.org/web/eoportal/satellite-missions/q/quess
- Sasaki, M., Fujiwara, M., Ishizuka, H. et al. Field test of quantum key distribution in the Tokyo QKD Network. 2011. Retrieved from https://arxiv.org/abs/1103.3566

## 第14章：后量子密码学

在过去的几章中，我们一直专注于量子密钥分发，或 QKD。这种成熟的技术在很长一段时间内仍然会保持相关性。与其相伴而来的是被称为*后量子密码学*的东西，这将是本章的主要主题。我们将探讨该主题的概述，[content]

在下一章中，我们将探讨量子计算和密码学在不久的将来所带来的许多其他令人兴奋的技术。

## 参考文献

- Elliott, C. The DARPA Quantum Network. 2004. Retrieved from arXiv:quant-ph/0412029
- Peev, M., Pacher, C., Alleaume, R. et al. The SECOQC quantum key distribution network in Vienna. New Journal of Physics, Volume 11, 2009.
- D. Stucki, M. Legre, F. Buntschu et al. Long term performance of the SwissQuantum quantum key distribution network in a field environment. New Journal of Physics, 13 123001, 2011.
- Kramer, H. QUESS (Quantum Experiments at Space Scale) / Micius. Retrieved from: https://directory.eoportal.org/web/eoportal/satellite-missions/q/quess
- Sasaki, M., Fujiwara, M., Ishizuka, H. et al. Field test of quantum key distribution in the Tokyo QKD Network. 2011. Retrieved from https://arxiv.org/abs/1103.3566

## 第14章：后量子密码学

在过去的几章中，我们一直专注于量子密钥分发，或 QKD。这种成熟的技术在很长一段时间内仍然会保持相关性。与其相伴而来的是被称为*后量子密码学*的东西，这将是本章的主要主题。我们将探讨该主题的概述，梳理后量子领域中最相关的加密方案的基本原理。

### 后量子密码学

为了澄清，*后量子密码学 PQC* 是一个科学领域，在其中为各种设备和场景研究新的抗量子（主要是）公钥算法解决方案。另一方面，*量子密码学*通常指的是使用量子密钥分发 QKD 结合当代加密技术，如本书前面所解释的那样。

现在，许多经典的密码算法要么依赖于整数分解（例如 RSA），要么依赖于离散对数（例如 ElGamal）。在当前计算范式下提供强大的安全性，但对于这些方法来说，近未来可能不会那么光明。毕竟，像*Shor 算法*这样的对当代加密方案构成威胁的算法已经被发现。随着功能强大的量子计算机的出现，需要实施一些有效的防御措施。第一道防线就是简单地增加当代加密算法中使用的密钥大小，例如从 128 位增加到 256 位（以及更高）。接下来是向完全内在抗量子的加密的过渡。同样，在这个新时代的密码方案被称为*后量子算法*。

未来最大的挑战之一是以集成量子抗性到现有设备中为形式出现的。由微软和谷歌等大公司开发的附加硬件密码处理器可能是潜在的解决方案。

当今的大多数哈希函数在很大程度上被认为对基于量子的攻击免疫。然而，一种称为*Grover 算法*的技术可能能够击败其中一些哈希函数。计算该算法时，本质上是在同时测试每个输入值以查看哪个是正确的，同时利用量子叠加。

大多数提出的 PQC 协议使用非对称方法。目前正在研究几种主要的 PQC 实现。我们接下来将探讨其中的一些。

### 基于哈希的密码学

基于哈希函数安全性构建的密码原语通常被称为*基于哈希的密码学*。这种密码学的变体目前仅限于数字签名方案。美国国家标准与技术研究所 NIST 于 2019 年宣布，其打算发布基于*Leighton-Micali 签名 LMS* 和*扩展 Merkle 签名方案 XMSS* 的有状态基于哈希的密码学的标准。我们很快将研究这两种方案。

在哈希方案的上下文中，状态是指记录的信息，通常是输入密码，不应重复使用。无状态签名方案在每次签名计算后不会请求信息/密码。

- **Merkle 签名方案 MSS。** 尽管这种方法可以追溯到 20 世纪 70 年代末，但它仍然被认为在计算上快速且对量子计算机具有抗性。Merkle 方案可以使用一个公钥对任意数量的消息（为 2 的幂）进行签名。Merkle 树是通过反复对节点对进行哈希处理，直到只剩下一个哈希为止，该哈希被称为 Merkle 根（见图 14-1）。

  ![](img/df9b6c40c665f58cab0c435444990c63_270_0.png)

  图 14-1. Merkle 树的基本布局。在此上下文中，连接指的是将十六进制字符组合的过程。通过修改数据节点 A、B、C 或 D 中的信息，上面的节点也会被更改，从而导致不同的 Merkle 根。

  该根节点成为公钥；它随后可用于验证所有其他节点。撇开消息签名不谈，包括*比特币*在内的许多加密货币都使用 Merkle 树方法，以其快速高效的数据验证能力著称。

- **扩展 Merkle 签名方案 XMSS, XMSS MT。** 于 2011 年引入，并且不出所料与 Merkle 签名方案有很多共同之处，XMSS 是该经典方案的更安全版本，通常也能产生更小的签名大小。*XMSS MT* 是该方案的更强大的多树变体，理论上可以签署无限数量的消息；与 XMSS 相比，该变体通常能更快地创建签名，但代价是文件更大（Hülsing & Gazdag）。

- **Leighton-Micali 签名 LMS。** 在 Merkle 方案的基础上改进，完整 LMS 方案的基本组件是*一次性 OT 签名方案*。它由 OT 密钥生成、签名和验证算法组成。在完整方案中，我们将这种一次性方案作为子程序与 Merkle 树结构结合起来，形成完整的有状态 LMS 签名方案。

- **SPHINCS/SPHINCS+。** 于 2015 年引入，*SPHINCS* 是一种无状态签名方案。SPHINCS 的一个小缺点是其相当大的签名大小为 41 千字节。该方案对量子计算机提供强大的安全性。SPHINCS+ 是对该方法的更新，其中之一就是解决了签名大小问题。即使是 SPHINCS+ 的最高安全实现，其签名大小也为 30 千字节；该方案在最低安全级别可低至八千字节（Bernstein & Hopwood et al., 2015）。

### 基于编码的密码学

这一分支的抗量子密码学依赖于纠错码 ECC。像加密和签名这样的大多数基本密码功能都可以使用编码理论概念实现。从消费者的角度来看，优化的基于编码的密码学在数据加密和解密期间都可能异常快速，使其非常适合电池寿命有限的移动设备（Engelbert, Overbeck, & Schmidt, 2007）。

- 1978 年的非对称*McEliece 密码系统*是基于编码方法的首次实现。该方案使用特定纠错码，即*二进制 Goppa*。Goppa 码基于模运算，其中达到特定目标值（即*模数*）的数字会再次回到零。最常见的现实生活例子是十二小时钟。这样的设备据说使用*模 12 算术*。除了 McEliece 密码系统之外，许多密码算法，包括 RSA 和 AES，都以各种方式利用模运算（Engelbert, Overbeck, & Schmidt, 2007）。

- Goppa 码虽然很稳健，但往往需要相当大的公钥。McEliece 方案的这一方面可能需要进一步开发。已经提出了其他更轻量级的代码来取代 Goppa 码提供的陷门函数，但没有一种被证明像它们那样能抵抗密码分析（Au, Eubanks-Turner, & Everson, 2013）。

