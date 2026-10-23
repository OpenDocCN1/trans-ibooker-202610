# 第 8 章：C/C++20 中的密码学库

本章的目的是提供一份全面的 C++ 库列表，这些库可以与 C++20 的新特性成功使用。当你需要访问特定功能的特定实现时，本章非常有用。你不需要搜索不同的在线资源，并且可以访问可供使用和改进的源代码。

## 密码学库概述

表 8-1 列出了最重要的密码学库。选择基于两个指标：执行时间和灵活性，以及基于开源许可证的源代码访问。能够访问源代码非常有用，因为你可以将自己的工作和算法与其他算法和实现进行比较，从而改进自己的工作。

**表 8-1. 主要 C/C++ 库**

| 库标题 | 开发者/行业 | 编程语言 | 开源 | 参考文献 |
|---|---|---|---|---|
| OpenSSL | OpenSSL Project | C | X | [1][2][3] |
| Crypto++ | Crypto C++ Project | C++ | X | [7][8] |
| Botan | Jack Lloyd | C++ | X | [5] |
| Libgcrypt | GnuPG Community | C | X | [9][10] |
| GnuTLS | Simon Josefsson, Nikos Mavrogiannopoulos | C | X | [11][12] |
| Cryptlib | Peter Gutmann | C | X | [13] |

对于每个库，我们将介绍密码学原语的最佳实现（例如密钥生成和交换、椭圆曲线密码学、公钥密码学、哈希函数、MAC 算法、分组密码等）。

## 哈希函数

表 8-2 显示了密码学库及其在不同哈希函数中的功能。在第 2 章中，我们提供了一个简单的 SHA-256 哈希函数的实现，你学习了如何从头开始实现哈希函数。

**表 8-2. 密码学库中哈希函数的存在情况**

| 库标题 | MD5 | SHA-1 | SHA-2 | SHA-3 | Whirlpool | GOST | BLAKE2 |
|---|---|---|---|---|---|---|---|
| OpenSSL | X | X | X | X | X | X | X |
| Crypto++ | X | X | X | X | X | X | X |
| Botan | X | X | X | X | X | X | X |
| Libgcrypt | X | X | X | X | X | X | X |
| GnuTLS | 该库代表 TLS、SSL 和 DTLS 协议的实现。 |  |  |  |  |  |  |
| Cryptlib | X | X | X | X | X | - | - |

在本节中，我们将从库中随机挑选一个哈希函数（例如 OpenSSL 中的 MD5 实现），并对实现提供一些评论。必须提到的是，为 MD5 哈希函数提供的实现已在 OpenSSL 中实现，并且将尊重 [4] 中的原始实现。首先要做的是下载文件 openssl-1.1.1g.tar.gz 并提取内容，以便访问源代码。提取后，导航到 crypto 文件夹，路径为 openssl-1.1.1g\crypto。这样，你就可以访问库中实现的所有密码算法的源代码文件。见图 8-1。





OpenSSL FIPS Object Module 2.0 (FOM) 也可供下载。它不再接收更新。它必须与 FIPS 兼容版本的 OpenSSL（1.0.2 系列）一起使用。新的 FIPS 模块目前正在开发中。

| KBytes | Date | File |
|---|---|---|
| 9306 | 2020-Apr-23 13:53:10 | openssl-3.0.0-alpha1.tar.gz (SHA256) (PGP sign) (SHA1) |
| 9571 | 2020-Apr-21 13:01:56 | openssl-1.1.1g.tar.gz (SHA256) (PGP sign) (SHA1) |
| 1457 | 2017-May-24 18:01:01 | openssl-fips-2.0.16.tar.gz (SHA256) (PGP sign) (SHA1) |
| 1437 | 2017-May-24 18:01:01 | openssl-fips-ecp-2.0.16.tar.gz (SHA256) (PGP sign) (SHA1) |

Figure 8-1. Downloading the openssl-1.1.1g.tar.gz file with source code

## MD5 Hash Function Overview

我们选择这个例子是因为它是一个简单的算法，所以很容易跟随和理解。MD5 的实现包含三个文件（两个 C/C++ 文件和一个头文件）以及一个包含三个用 PERL 语言编写的文件的 ASM 文件夹。PERL 文件是对四个平台 586、x86、x64 和 sparc 的优化。参见图 8-2。

![](img/c3d9424f64735276d661e7aba725b2a2_163_0.png)

Figure 8-2. Example of a MD5 hash in action for a file

## Public Key Cryptography

大多数库都包含 PKCS（Public Key Cryptography Standards）不同标准的实现，并且经过了充分测试（参见表 8-3）。

**Table 8-3.** Existence of Public Key Cryptography Protocols Within Cryptography Libraries

| Library Title | PKCS#1 | PKCS#5 | PKCS#8 | PKCS#12 | IEEE P1363 | ASN.1 |
|---|---|---|---|---|---|---|
| OpenSSL | X | X | X | X | - | X |
| Crypto++ | X | X | X | - | X | X |
| Botan | X | X | X | - | X | X |
| Libcrypt | X | X | X | X | X | X |
| Cryptlib | X | X | X | X | - | - |

为了使用 OpenSSL 进行公钥加密，请按照以下示例查看工作流程。假设有两个用户 Alice 和 Bob 相互通信。通信工作流程如下。

- Step 1. Alice 生成一个 2048 位的私钥，alicePrivKey.pem。参见图 8-3。

```
openssl genrsa -out alicePrivKey.pem 2048
```

![](img/c3d9424f64735276d661e7aba725b2a2_164_0.png)

Figure 8-3. Generating a private key

- Step 2. Alice 提取公钥 alicePublicKey.pem 并发送给 Bob。参见图 8-4。

```
openssl rsa -pubout -in alicePrivKey.pem -out alicePublicKey.pem
```

![](img/c3d9424f64735276d661e7aba725b2a2_165_0.png)

*Figure 8-4. Extracting the public key*

- Step 3. Bob 加密消息并将 BobMessageToAlice.txt 发送给 Alice。

```
openssl rsautl -encrypt -in cleartext -out encryptedWithAlicePubKey -inkey alicePublicKey.pem -pubin
```

- Step 4: Alice 从 Bob 那里解密消息。

```
openssl rsautl -decrypt -in encryptedWithAlicePubKey -inkey alicePrivKey.pem
```

## Elliptic-Curve Cryptography (ECC)

基于椭圆曲线的最常用的密钥交换协议之一是 ECDH（Elliptic Curve Diffie-Hellman）；参见表 8-4。该协议的目的是在加密过程中设置一个共享的秘密密钥，而无需在通信过程中直接将其发送给每个通信伙伴。

**Table 8-4.** Existence of Elliptic-Curve Cryptography Within Cryptography Libraries

| Library Title | NIST | SECG | ECDSA | ECDH | GOST R 34.10 |
|---|---|---|---|---|---|
| *OpenSSL* | X | X | X | X | X |
| *Crypto++* | X | X | X | X | - |
| *Botan* | X | X | X | X | X |
| *Libcrypt* | X | X | X | X | X |
| *Cryptlib* | X | X | X | X | - |

为了避免协议背后的数学装置，我们将总结协议的工作流程，以便您清楚地了解在通信伙伴（Alice 和 Bob）之间交换的域参数：

- Alice 使用域的参数生成私钥和公钥。
- Bob 使用上面设置的域参数生成私钥和公钥。
- 两个用户交换他们的公钥。
- Alice 使用 Bob 的公钥计算，共享函数以一个共享的秘密为特征，被称为 B 的派生密钥。
- Bob 用 Alice 的公钥做同样的事情。共享函数以一个共享的秘密为特征，被称为 A 的派生密钥。
- Alice 现在使用 Bob 的派生密钥加密消息。
- Bob 使用 Alice 的派生密钥加密消息。
- 两个用户都可以使用自己的私钥解密消息。

### Creating ECDH Keys

首先，很重要的是检查你的机器上对 ECDH 密钥的 OpenSSL 支持情况。为了实现这些主要任务，运行命令 openssl ecparam -list_curves（参见图 8-5）。该命令将提供一个可用的曲线完整列表。其中大多数都已根据其标准正确实现。它们在 OpenSSL 中的实现以及使用 C++20 新特性的最新更新使它们易于跟随。

![](img/c3d9424f64735276d661e7aba725b2a2_166_0.png)

Figure 8-5. Getting a list of curves

有一种更快的方法来创建密钥对，使用以下命令（参见图 8-6）：

```
openssl ecparam -name prime256v1 -genkey -noout -out key.pem
```

![](img/c3d9424f64735276d661e7aba725b2a2_167_0.png)

**Figure 8-6.** Generating a keypair

这将输出类似

```
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIKPvG4c8qipI+aZmV3SVnYCZ/QxVfsbJ1CFk4H1uHhFJoAoGCCqGSM49
AwEHoU0DQgAEj0Ztq0qt7r8aBkdxVvXxoJC4BdvoPIVBHlsIfw7+GIWmOAJNzFg5
IHbZQF6OhNdWguSl4/Mug24NvGG4PapIhg==
-----END EC PRIVATE KEY-----
```

如果您想查看 EC 参数的详细信息，请运行以下命令：`openssl ec -in key.pem -text -noout`。该命令将输出类似图 8-7 的内容。

![](img/c3d9424f64735276d661e7aba725b2a2_167_1.png)

**Figure 8-7.** Showing the details of the EC parameter

## OpenSSL

### Configuration and Installing OpenSSL

为了配置和正确安装 OpenSSL，根据使用的操作系统平台，请参考以下各节并相应地执行步骤。

#### Installing OpenSSL on Windows 32/64

- Step 1: Download the binaries for OpenSSL [3]. Download the latest version of OpenSSL Windows Installer by going to https://slproweb.com/products/Win32OpenSSL.html. Scroll down until you reach the Download Win32/Win64 OpenSSL option（参见图 8-8）。

![](img/c3d9424f64735276d661e7aba725b2a2_168_0.png)

Figure 8-8. Download section of OpenSSL

- Step 2: Double click and run Win64OpenSSL-1_1_1g.exe（参见图 8-9）。

![](img/c3d9424f64735276d661e7aba725b2a2_168_1.png)

Figure 8-9. Setup of OpenSSL

- Step 3: Accept the license agreement and click Next. See Figure 8-10.

Figure 8-10. OpenSSL license agreement

- Step 4: Specify the path where it should be installed and click Next. See Figure 8-11.

Figure 8-11. Setting up the path where OpenSSL will be installed

- Step 5: Leave the screen as is and click Next. See Figure 8-12.

![](img/c3d9424f64735276d661e7aba725b2a2_170_0.png)

Figure 8-12. The location of the program shortcuts

![](img/c3d9424f64735276d661e7aba725b2a2_170_1.png)

- Step 6. Leave everything as is and click Next. See Figure 8-13.

![](img/c3d9424f64735276d661e7aba725b2a2_171_0.png)

**Figure 8-13.** *Additional tasks to perform*

- Step 7: You’re ready for installation so click Install. See Figure 8-14.

![](img/c3d9424f64735276d661e7aba725b2a2_171_1.png)

**Figure 8-14.** *Acknowledgment of the installation process and settings*

- Step 8: Installation progress. Remember that if you haven’t installed Microsoft Visual C++ Redistributable (x64) it will ask you to install it. See Figure 8-15.

![](img/c3d9424f64735276d661e7aba725b2a2_172_0.png)

**Figure 8-15.** *OpenSSL installation progress*

- Step 9: Finishing the process of installation. Leave everything as is and click Finish. See Figure 8-16.

![](img/c3d9424f64735276d661e7aba725b2a2_172_1.png)

**Figure 8-16.** *Completing the process of installation*





步骤 10：配置并设置 OpenSSL 的环境变量。

步骤 11：运行环境变量。转到系统属性并单击环境变量。见图 8-17。

![](img/c3d9424f64735276d661e7aba725b2a2_173_0.png)

图 8-17. 系统属性

步骤 11：OpenSSL 的环境变量将添加到系统变量中。单击图 8-18 所示的新建按钮。

图 8-18. 环境变量

步骤 12：配置 OPENSSL_CONF 变量。见图 8-19。

![](img/c3d9424f64735276d661e7aba725b2a2_174_0.png)

![](img/c3d9424f64735276d661e7aba725b2a2_174_1.png)

图 8-19. 一个新的系统变量

步骤 13：相应地配置和修改路径变量。选择系统变量中的 Path 变量并单击编辑。见图 8-20。

![](img/c3d9424f64735276d661e7aba725b2a2_175_0.png)

图 8-20. 环境变量 ▶ 路径变量

步骤 14：在编辑环境变量部分，单击新建和浏览。见图 8-21。

![](img/c3d9424f64735276d661e7aba725b2a2_176_0.png)

**图 8-21.** *编辑环境变量*

步骤 15：选择 OpenSSL bin 文件夹的路径并单击确定。新的路径已成功添加。关闭所有内容。如果您打开了命令窗口，请关闭它并重新打开以使更新生效。否则将无法工作。见图 8-22。

图 8-22. 验证 OpenSSL 的路径已添加

步骤 16：打开命令 (cmd.exe)。运行 openssl 命令。如果窗口中出现 OpenSSL ► 提示符，则意味着您已获得成功的第一个迹象。见图 8-23。

![](img/c3d9424f64735276d661e7aba725b2a2_177_0.png)

![](img/c3d9424f64735276d661e7aba725b2a2_177_1.png)

图 8-23. 检查 OpenSSL，第一步

步骤 17：运行第二个命令 version。确保一切都已正确设置。如果版本和日期如图所示返回，如图 8-24 所示，您可以宣布自己成功。

![](img/c3d9424f64735276d661e7aba725b2a2_178_0.png)

图 8-24. 检查 OpenSSL，第二步

# 在 Linux 上安装 OpenSSL – Ubuntu 版本

通常，OpenSSL 已预装在 Linux – Ubuntu 上。对于本分步指南，我们使用了 Ubuntu 18.04.3 LTS 版本（代号：bionic）。

在继续安装之前，通过在终端中运行 `openssl version -a` 检查已安装的 OpenSSL 版本。见图 8-25。

![](img/c3d9424f64735276d661e7aba725b2a2_178_1.png)

图 8-25. 检查 OpenSSL 版本

如果看不到它，则意味着 OpenSSL 未安装或未正确配置。请按如下方式安装和配置 OpenSSL。

步骤 1：通过在终端中运行以下命令将 Ubuntu 系统更新到最新软件包：`sudo apt-get update && sudo apt-get upgrade`。您将被要求用 Y 或 N 回答以继续。选择 Y（是）。见图 8-26。

![](img/c3d9424f64735276d661e7aba725b2a2_179_0.png)

图 8-26. 使用最新软件包更新 Ubuntu 系统

步骤 3：安装编译所需的软件包。这是非常重要的一步。请谨慎操作。命令为 `sudo apt install build-essential checkinstall zlib1g-dev -y`。见图 8-27。

![](img/c3d9424f64735276d661e7aba725b2a2_179_1.png)

图 8-27. 安装编译所需的软件包

步骤 4：下载 OpenSSL。撰写本章时，版本为 1.1.1g。为此，请按照以下命令以及图 8-28 和图 8-29 所示进行操作。

```bash
cd /usr/local/src/
```

和

```bash
sudo wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
```

![](img/c3d9424f64735276d661e7aba725b2a2_180_0.png)

图 8-28. OpenSSL 安装位置

![](img/c3d9424f64735276d661e7aba725b2a2_180_1.png)

图 8-29. 获取正确的安装包

步骤 5：提取下载的文件。要实现此目的，请使用以下命令（如图 8-30 所示）：

```bash
sudo tar -xf openssl-1.1.1g.tar.gz
```

![](img/c3d9424f64735276d661e7aba725b2a2_181_0.png)

图 8-30. 提取下载的文件

步骤 6：导航到文件已提取的目录。见图 8-31。

![](img/c3d9424f64735276d661e7aba725b2a2_181_1.png)

图 8-31. 提取文件的位置

步骤 7：使用以下命令安装 OpenSSL（如图 8-32 至 8-35 所示）：

```bash
sudo ./config -prefix=/usr/local/ssl -openssldir=/usr/local/ssl shared zlib
```

![](img/c3d9424f64735276d661e7aba725b2a2_182_0.png)

图 8-32. OpenSSL 安装

```bash
sudo make
```

![](img/c3d9424f64735276d661e7aba725b2a2_182_1.png)

图 8-33. 运行 sudo make

```bash
sudo make test
```

![](img/c3d9424f64735276d661e7aba725b2a2_183_0.png)

图 8-34. 运行 sudo make test

```bash
sudo make install
```

![](img/c3d9424f64735276d661e7aba725b2a2_183_1.png)

图 8-35. 运行 sudo make install

步骤 8：配置 OpenSSL 共享库。首先，您需要导航到 `/etc/ld.so.conf.d` 目录并手动创建配置文件 `openssl-1.1.1g.conf`。命令如下（如图 8-36 至 8-39 所示）：

```bash
cd /etc/ld.so.conf.d/
```

![](img/c3d9424f64735276d661e7aba725b2a2_184_0.png)

图 8-36. OpenSSL 共享库配置

```bash
sudo nano openssl-1.1.1g.conf
```

![](img/c3d9424f64735276d661e7aba725b2a2_184_1.png)

图 8-37. 编辑配置文件

![](img/c3d9424f64735276d661e7aba725b2a2_185_0.png)

图 8-38. 编辑配置文件

```bash
sudo ldconfig -v
```

![](img/c3d9424f64735276d661e7aba725b2a2_185_1.png)

图 8-39. 验证配置文件

步骤 9：配置 OpenSSL 二进制文件。此步骤非常敏感。您将二进制文件或新安装的 OpenSSL 版本（位于 `/usr/local/ssl/bin/openssl`）覆盖默认的 OpenSSL 二进制文件。

首先，通过运行以下命令创建二进制文件的备份：

```bash
sudo mv /usr/bin/c_rehash /usr/bin/c_rehash.backup
```

其次，使用 vim 编辑 `/etc/environment` 文件。见图 8-40。

![](img/c3d9424f64735276d661e7aba725b2a2_187_0.png)

*图 8-42.* 验证安装

## Botan

Botan [5] 代表另一种功能强大的库，可像 OpenSSL 一样在命令行中使用。算法种类繁多，包含非常强大和现代的实现（包括 C++20 特性）。Botan 与其他库的区别在于为传输层安全（TLS）协议实现的模块。Botan 实现的功能使其成为专业人士寻求灵感和指导的真正候选者。文档易于遵循。

命令和说明与 OpenSSL 的相同，只是与公钥算法相关的细微差异。

## CrypTool

使用 C++ 开发的用于密码学的优秀软件产品是 CrypTool (CT) [6]，版本 1。版本 CT1 的最新稳定版本为 1.4.41，可从 CrypTool 官方网站¹ 下载。下载后，启动可执行文件并按照说明进行安装。打开 CT1 后，主窗口如下图 8-43 所示。

¹ [www.cryptool.org/en/ct1-downloads](http://www.cryptool.org/en/ct1-downloads)

![](img/c3d9424f64735276d661e7aba725b2a2_188_0.png)

图 8-43. CrypTool 1 中的主窗口

我们将要查看的第一个示例是经典密码，凯撒。它可从 加密/解密 ▶ 对称（经典） ▶ 凯撒/Rot-13... 中选择。在选择凯撒密码之前，先关闭 startingexample-en.txt 窗口并从 文件 ▶ 新建 打开一个新窗口。在打开的窗口中，输入句子 This is an example of Caesar cipher using CrypTool 1。见图 8-44。

该窗口包含密码的简短描述。请注意，Rot-13 是凯撒密码的特殊情况，将特定字母移动 13 个位置（考虑到英语字母表的字母数为 26，则其一半为 13，因此得名 Rot-13）。使用默认变体，凯撒。在右侧，您可以选择字母表第一个字母 A 的索引；它可以是 0 或 1。此外，您应选择密钥，表示特定字母在字母表中向右移动的位置数。密钥可以是字母表字母或数字。选择字符选项并假设密钥为 M。图 8-45b 显示了所选加密部分属性中的更改。请注意 A 映射到 M（A 的位置为 0，移动 12 个位置，即 0+12=12；英语字母表的第 12 个字母是 M）等等。现在按 *Encrypt* 按钮。您应获得图 8-46 所示的结果。

![](img/c3d9424f64735276d661e7aba725b2a2_190_0.png)

*图 8-46.* 使用凯撒密码进行加密

请注意，密码不区分大小写。此类附加设置可通过按 *Key Entry: Caesar/ROT-13* 窗口中的 *Text Option* 访问，可在图 8-47 的左侧窗口中看到。在此窗口中，保持未在字母表中出现的字符不变。请注意，字符 1 和 . 甚至空格都没有被加密。此外，您可以选择大写敏感性，可以扩展字母表，并可以设置统计用途的参考（见图 8-47 的右侧窗口）。





![](img/c3d9424f64735276d661e7aba725b2a2_191_0.png)

## 图 8-47. 文本选项

现在让我们回到示例。关闭 Unnamed-1 窗口（或确保强调的窗口是 Caesar 加密的 <Unnamed1>，密钥 <M, KEY OFFSET: 0>），并对图 8-47 中得到的 Caesar 加密结果进行解密。为此，从菜单中选择 Caesar 密码，并使用与加密时相同的设置。请注意 Caesar 是一种对称密码，这意味着加密和解密使用相同的密钥，因此将密钥输入设为字母字符，选择 M 并按下 Decrypt 按钮。结果如图 8-48 所示。

# 第 8 章　C/C++20 中的密码学库

![](img/c3d9424f64735276d661e7aba725b2a2_192_0.png)

我们接下来要查看的下一个加密系统是 RSA。选择 *Encrypt/Decrypt* ➤ *Asymmetric* ➤ *RSA Demonstrator*。*RSA Demonstrator* 窗口应如图 8-49 所示。

![](img/c3d9424f64735276d661e7aba725b2a2_192_1.png)

保留同时计算公钥和私钥的默认选项。选择该方案的参数。你可以自行提供两个素数，也可以利用生成器生成两个素数。点击 Generate prime numbers 按钮。窗口应如图 8-50 左侧窗口所示。

![](img/c3d9424f64735276d661e7aba725b2a2_193_0.png)

## 图 8-50. RSA 的素数生成器

在这里，你可以在三种素数生成器之间进行选择。选择 Fermat Test，将 p 和 q 的下限设为 2¹²，上限设为 2¹⁵，并选择独立素数，然后按下 Generate prime numbers 按钮（图 8-50 右侧窗口）。要使用这些素数，请按下 Apply primes 按钮。生成素数后，注意公钥和密钥值已被计算（图 8-50 左侧窗口）。保留默认公钥为 2¹⁶ + 1，检查 Input 字段的文本选项，并输入以下内容：This text is encrypted using RSA。按下 Encrypt 按钮。结果应如图 8-51 所示。

## 图 8-51. 使用 RSA 加密的文本

现在，要解密，你不应关闭窗口，并且需要稍微小心一些。解密时，将字段 Encryption 中得到的结果文本复制到 ciphertext c[i]=m[i]^e (mod N)。你的加密文本应如下所示（并显示在图 8-52 左侧窗口中）：

```
212901699 # 120812360 # 045225910 # 168182322 # 103916866 # 349246149 #
027823531 # 310207436 # 009232756 # 131763739 # 089946941
```

此外，检查 *Input* 的 *numbers*，然后粘贴。随后按下 *Decrypt* 按钮。解密结果显示在图 8-52 右侧窗口中。你应获得与之前加密相同的明文。

![](img/c3d9424f64735276d661e7aba725b2a2_195_0.png)

这只是使用 CrypTool 1 的两个简单示例。它提供了更多加密方案和示例，并且可用于攻击模拟或收集不同的统计信息。

## 结论

在本章中，我们提供了 C++ 库的简要列表，并展示了如何在 Windows 操作系统或 Ubuntu 上安装它们。最有用的 C++ 开发库是 OpenSSL、Botan 和 CrypTool。

到本章结束时，你学习了以下内容：

- 如何访问最重要的开源密码学库和框架
- 主要密码学操作的工作原理以及如何与这些库和框架交互
- 如何访问其源代码以比较算法的实现
- 如何从其他专业开发人员（例如 OpenSSL、Botan 等）学习开发密码学算法的最佳实践

## 参考文献

[1] OpenSSL: Cryptography and SSL/TLS Toolkit. Available online: www.openssl.org/.

[2] OpenSSL TLS/SSL and Crypto Library. Available online: https://github.com/openssl/openssl.

[3] Win32/Win 64 OpenSSL Installer for Windows. Available online: https://slproweb.com/products/Win32OpenSSL.html.

[4] OpenSSL Sources. Available online: www.openssl.org/source/.

[5] Botan: Crypto and TLS for Modern C++. Available online: https://botan.randombit.net/.

[6] CrypTool Portal. Available online: www.cryptool.org/en/.

[7] Crypto++. Available online: https://cryptopp.com/.

[8] Crypto++ Manual. Available online: https://cryptopp.com/docs/ref/.

[9] Libcrypt. Available online: www.gnupg.org/related_software/libgcrypt/.

[10] Libcrypt. Available online: www.gnupg.org/documentation/manuals.html.

[11] GnuTLS. Available online: https://gnutls.org/.

[12] GnuTLS Documentation. Available online: https://gnutls.org/documentation.html.

[13] Cryptlib. Available online: www.cryptlib.com/.

## 第二部分

## 专业密码学
