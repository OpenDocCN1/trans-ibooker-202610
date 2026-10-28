

45 MINUTES

## Lab Exercise 11.04: Rainbow Table Attacks on Windows Passwords with ophcrack

彩虹表攻击是一种权衡。与暴力攻击相比，彩虹表攻击使用更少的处理资源（暴力攻击可能永远持续下去），而使用更多的存储空间（暴力攻击可以在没有任何存储的情况下完成）。

与字典攻击相比，彩虹表攻击使用更多的处理资源（要处理的内容远不止一个简单的词表），而使用更少的存储空间（尽管彩虹表明显大于词表，但它们采用了一种压缩形式，使得与词表大小相同的彩虹表可以容纳极大量的额外数据，这些数据在处理时会显现出来）。

彩虹表涉及一种称为约简函数的算法，该算法将哈希映射为可能为实际明文密码的东西。这并不意味着它会反转哈希，因为这是无法完成的。彩虹表在哈希函数和约简函数之间交替，以产生交替的哈希和明文的链。

例如，如图 11-7 所示，明文字符串 Jonathan 可以通过哈希函数处理以产生 123456。然后，123456 会通过约简函数处理以产生 Scott。然后，Scott 会通过刚使用过的同一哈希函数处理以产生 abcdef。最后，abcdef 通过不同的（稍后会解释原因）约简函数处理以产生 Weissman。

![](img/6e921d997751f0a0f87e5170ac9657a1_342_0.png)

FIGURE 11-7 Hashing and reducing

约简函数可以根据哈希（本身就是一个值）生成一个数学值，然后将该值编码为字母、数字和符号。如图 11-7 所示，123456 被约简为一个值（类似于 536），该值被编码为 Scott。

约简函数可以简单到取一个四位哈希（以十六进制、基数 16 表示）并删除前两位数字以产生明文。更进一步，这两位数字可以通过一些数学运算映射到 ASCII/Unicode（参见 www.asciitable.com）。

使用 rtgen 等工具生成彩虹表涉及计算具有特定链长度的若干链（行）。用于字典攻击的词表最多可存储 10,000 个明文值或哈希值，最多可破解 10,000 个密码。然而，一个彩虹表只需存储 5,000 条链的第一个和最后一个列（起点和终点），即 10,000 个条目，与词表相同数量的条目，却可以破解任意数量的密码：100,000、100 万、10 亿个或更多！

彩虹表特定于为其创建的哈希函数，这意味着，例如，SHA-512 表只能破解 SHA-512 哈希。

彩虹表由许多这样的链组成，使用不同的约简函数在表的每个位置进行约简；因此，如果在两条或多条链中发生哈希碰撞，只要碰撞不在每条链的同一位置，链就不会合并。虽然哈希函数应能抵抗不同输入产生相同输出，但约简函数不具备该特性。因此，如果相同的约简函数将两个不同的明文转换为相同的哈希，则从该点开始链将相同，从重复的哈希开始，这将转化为宝贵处理时间的浪费。

除了增加给定表大小的正确破解概率（因为会有更多唯一条目而非重复条目）之外，使用多个约简函数几乎使查找速度翻倍。

想象一个彩虹表看起来像图 11-8 所示，有三行七列。如前所述，只有第一列和最后一列被存储。

| Start | H | R1 | H | R2 | H | R3/End |
|---|---|---|---|---|---|---|
| qwerty | fec1 | apple | e6c1 | dwell | 8844 | Munson |
| password | 9992 | happy | 4432 | computer | 8888 | Orange |
| bob | a2b3 | Rochester | abcd | dog | a02f | RIT |

FIGURE 11-8 Rainbow table

实际上，彩虹表可以有数百甚至数千行（链的数量）和列（每条链的长度）。

在哈希密码数据库中，我们看到 Linux 系统中的 root 账户或 Windows 系统中的 Administrator 账户的哈希值为 abcd。这里使用四位哈希仅用于说明目的。

首先，对表中最后一次使用的约简在哈希上计算，并将结果与表中最后一列的每个条目进行比较，如图 11-9 所示。输出“edX”在最后一列的任何行中都不出现。

| Start | H | R1 | H | R2 | H | R3/End |
|---|---|---|---|---|---|---|
| qwerty | fec1 | apple | e6c1 | dwell | 8844 | Munson |
| password | 9992 | happy | 4432 | computer | 8888 | Orange |
| bob | a2b3 | Rochester | abcd | dog | a02f | RIT |

![](img/6e921d997751f0a0f87e5170ac9657a1_344_0.png)

FIGURE 11-9 The last reduction function

如果测试失败，而它确实失败了，则哈希会经过最后两个约简函数以及中间的哈希函数，如图 11-10 所示。

并且以这种方式输入哈希函数，而不是仅仅 Rochester，如果没有使用盐，哈希就会不同。

假设 Rochester 和盐的哈希值为 efgh。efgh 在此彩虹表中找不到，因为此彩虹表是在没有任何盐的情况下生成的。因此，efgh 不会导致密码 Rochester，使用此彩虹表的攻击将不成功。

不过，关于盐有某些重要规则。首先，盐应该是唯一的，不仅在数据库中，而且在全球范围内（通过使用独特的方式生成它们）。如果使用特定盐生成彩虹表，最多只能攻击一个密码（该密码应用了该特定盐）。唯一的盐还通过使相同密码的加盐哈希实例彼此不同，从而保护常用密码，或那些在多个站点使用相同密码的人。其次，盐还应该是随机且不可预测的，这样攻击者就无法基于知道盐是基于诸如用户 ID 之类的东西来构建少量狭窄的彩虹表。第三，盐应该很长。由于盐不需要记忆，它们可以使成功攻击所需的彩虹表大小变得 prohibitively 大，而不会给用户带来任何负担。现在当你登录系统时，你的密码会在哈希之前与盐连接，如图 11-13 所示。

像以前一样，哈希与密码数据库中存储的哈希进行比较。如果两个哈希匹配，用户将被验证。

你认为盐对预计算的字典文件有效吗？如果该文件包含没有盐的预计算哈希，当然有效。现在该文件毫无价值，因为所有这些哈希都是在没有任何盐的情况下计算的。如果该文件包含将被哈希的明文条目呢？也是如此。如果每个用户的盐都不同，那么大型字典文件需要在哈希之前将所有盐应用于每个明文。这可能是数千或数百万个盐需要应用于字典文件中的每个条目，这对攻击者来说是不切实际的。

然而，当攻击者想要通过暴力攻击或字典攻击破解单个密码时，盐不会提供任何保护，因为它们以明文形式存储，并且可以轻松地将用户的唯一盐添加到字典文件的每个条目中。盐并非旨在防止单个密码被破解，而是为了防止使用彩虹表，并防止泄露的密码数据库被用于后续攻击。

## Learning Objectives

在本实验练习中，你将使用第三种攻击类型攻击密码。在本实验练习结束时，你将能够

- Understand how rainbow tables work
- Understand how salt foils rainbow tables
- Perform a rainbow table attack with ophcrack

## Lab Materials and Setup

本实验所需的材料有

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A web browser with an Internet connection
- The Windows 10 VM you created in Chapter 1

## Let’s Do This!

彩虹表攻击与字典攻击或暴力攻击非常不同，正如你将在本实验练习中看到的那样。使用 ophcrack 和预计算的彩虹表，你将破解使用 Mimikatz 提取的 Windows 密码哈希。

1a–1n

## Step 1 Download rainbow table files and install them to be used in ophcrack.

- a. Go to https://ophcrack.sourceforge.io/, click the Download menu item at the top, and click the download button for Windows (portable). The download from SourceForge will start automatically.
- b. Extract the ZIP file and install the software.
- c. In the uncompressed folder, open the x86 folder if you’re on a 32-bit

