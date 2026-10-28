

host machine

# Let’s Do This!

在 Ubuntu VM 中，按 CTRL-ALT-T 打开一个新的终端。执行每条命令后按 ENTER。

1a–1k

## Step 1 Use custom and default Snort configuration files.

a. 在使用 Snort 的默认配置文件和规则之前，让我们先编写一个简单的配置文件以便开始：

```
sudo gedit /etc/snort/snort2.conf
```

使用 gedit 文本编辑器创建并编辑存储在 /etc/snort 中的 snort2.conf 文件，这里就是默认 snort.conf 文件所在的位置。对于类型为 ipvar 的变量 HOME_NET，如果你的网络 ID 为 192.168.1.0/24，则使用该值。否则，根据你的 IP 地址方案进行修改。route 命令会显示你的网络 ID。将以下行添加到配置文件中（第 3 行使用你的网络 ID）：

```
preprocessor frag3_global: max_frags 65536
include classification.config
ipvar HOME_NET 192.168.1.0/24
var RULE_PATH /etc/snort/rules
include $RULE_PATH/local.rules
```

点击屏幕右上角的红色 X，然后点击保存按钮。这会在 Snort 启动时使用的配置文件中添加一组最少的行。

包含了一个默认的预处理器，以消除 “WARNING: No preprocessors configured for policy 0.” 的消息。你可以在 http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node17.html 上阅读有关预处理器的更多信息，包括此特定预处理器。

文件 classification.config 已被包含。

以下内容摘自 http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node16.html：

include 关键字允许在 Snort 命令行上指定的 snort.conf 中包含其他 snort 配置文件。它的工作方式类似于 C 编程语言中的 #include，读取命名文件的内容并将其内容添加到文件中的 include 语句出现的位置。

要在你的系统上查看 classification.config 文件，输入

```
gedit /etc/snort/classification.config
```

然后按 ENTER。

对于类型为 var 的变量 RULE_PATH，将其值设为 /etc/snort/rules。include 语句会在 /local.rules 之前展开该路径（/etc/snort/rules），以产生规则文件的绝对引用。使用变量作为规则路径允许你对多个引用使用该变量，并且如果需要，只需更改 RULE_PATH 变量，而不必更改每个 include 语句。

查看完 classification.config 后关闭 gedit。

b. 输入以下命令：

```
sudo gedit /etc/snort/rules/local.rules
```

以下内容摘自文件中的注释部分：

```
This file intentionally does not come with signatures. Put your local additions here.
```

在单行中添加以下规则。不要按 ENTER 来换行。文本如有必要会自动换行到下一行。

```
alert icmp any any -> $HOME_NET any (msg:"ICMP detected!"; sid: 1000052; rev:1; classtype:icmp-event;)
```

Snort 规则分为两部分：规则头和规则选项。规则头出现在左括号之前，包含规则动作、协议、源 IP 地址、源端口、方向、目标 IP 地址和目标端口。头部回答了关于分析数据包的“谁？”这个问题。

此规则以 “alert” 开始，这是满足条件时要采取的动作。

以下内容摘自 http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node29.html：

- 1. alert 使用所选的告警方法生成告警，然后记录数据包。
- 2. log 记录数据包。
- 3. pass 忽略数据包。
- 4. drop 阻止并记录数据包。
- 5. reject 阻止数据包，记录它，然后如果协议为 TCP 则发送 TCP 重置，如果协议为 UDP 则发送 ICMP 端口不可达消息。
- 6. sdrop 阻止数据包但不记录它。

与 alert 规则和 log 规则相反，pass 规则将匹配不需要生成告警或日志的良性流量（例如，应该由 Snort 忽略的网络上的漏洞扫描器）。

在上一页的规则头中，icmp 匹配封装在其中的 ICMP 数据包。

第一个 **any** 指任何源 IP 地址。

第二个 **any** 指任何源端口。

接下来是方向运算符——在本例中，表示匹配从箭头左侧的源到箭头右侧的目标的流量。还有一个双向运算符 <>，指示 Snort 分析来自每一侧（源和目标）的流量。

第三个 **any** 指任何目标 IP 地址。

第四个 **any** 指任何目标端口。

所有这些都属于 Snort 规则头。

如果规则头回答“谁？”这个问题，那么规则选项则回答“什么？”这个问题

表 13-1 显示了通用规则选项关键字及其描述，详细内容见 http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node31.html。

| Keyword | Description |
|---|---|
| msg | msg 关键字告诉日志和告警引擎在数据包转储或告警中打印的消息。 |
| reference | reference 关键字允许规则包含对外部攻击识别系统的引用。 |
| gid | gid 关键字（生成器 id）用于标识当特定规则触发时生成事件的 Snort 的哪个部分。 |
| sid | sid 关键字用于唯一标识 Snort 规则。 |
| rev | rev 关键字用于唯一标识 Snort 规则的修订版本。 |
| classtype | classtype 关键字用于将规则归类为检测属于更一般攻击类别的攻击。 |
| priority | priority 关键字为规则分配严重级别。 |
| metadata | metadata 关键字允许规则编写者在规则中嵌入其他信息，通常采用键值格式。 |

TABLE 13-1 General Rule Option Keywords from the Snort Manual

让我们再看一下我们的 Snort 规则：

```
alert icmp any any -> $HOME_NET any (msg:"ICMP detected!"; sid:1000052; rev:1; classtype:icmp-event;)
```

规则选项部分以消息 “ICMP detected!” 开始，这是将在告警中显示并被记录的消息。

sid（Snort ID）唯一标识 Snort 规则。小于 100 的 ID 保留供将来使用。100 到 999,999 之间的 ID 为规则

你会注意到有八个 ICMP 告警（四个 Echo 请求和四个 Echo 回复），如图 13-5 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_419_0.png)

# FIGURE 13-5 Snort alerts generated

按 CTRL-C 退出。注意输出末尾的所有信息，包括以下部分：

- - 数据包处理的运行时间
- - 处理的数据包数量
- - 内存使用摘要
- - 数据包 I/O 总计
- - 按协议的细分（包括重建的数据包）
- - 动作统计
- - Frag3 统计
- - 流统计
- - SMTP 预处理器统计
- - Dcerpc2 预处理器统计
- - SIP 预处理器统计

e. 输入以下命令查看日志文件的名称：

```
sudo ls -l /var/log/snort
```

f. 在 Wireshark 中打开日志文件：

```
sudo wireshark /var/log/snort/snort.log.[numbers seen
```

从 Windows 10 VM 或 host machine 对 Ubuntu VM 执行 ping。

你会注意到有八个 ICMP 告警（四个 Echo 请求和四个 Echo 回复）。按 CTRL-C 退出。**fast** 选项已被 **full** 替换。

- i. 输入以下命令：
- ```
sudo gedit /var/log/snort/alert
```
- 注意告警文件中每个告警的完整 IP 头（尽管排除了一些字段）。

- j. 删除告警文件并重新启动 Snort，同时使用 **fast** 和 **full with two -A** 选项：
- ```
sudo rm /var/log/snort/alert
sudo snort -A console -A fast -A full -c /etc/snort/snort2.conf -i ens33
```
- 从 Windows 10 VM 或 host machine 对 Ubuntu VM 执行 ping。
- 你会注意到有八个 ICMP 告警（四个 Echo 请求和四个 Echo 回复）。按 CTRL-C 退出。现在同时使用了 **full** 和 **fast** 选项。

- k. 输入以下命令：
- ```
sudo gedit /var/log/snort/alert
```
- 同时指定 **fast** 和 **full** 选项后，你现在会在每个单独的告警中看到两者的输出（一个接一个）。

2a

2c, 2d

## Step 2 Examine Snort’s default rules.

- a. 现在让我们看看随 Snort 提供的规则：
- ```
ls -l /etc/snort/rules
```
- 注意不同类别的规则，每个类别都在自己的 .rules 文件中，如图 13-7 所示。

