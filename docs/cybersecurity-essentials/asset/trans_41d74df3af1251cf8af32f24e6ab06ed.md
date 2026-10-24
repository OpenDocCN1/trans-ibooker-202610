

![](img/2459befd842d6fed299fabe6da3512d0_386_0.png)

# FIGURE 14.16 Rule Paths Changed

13. 最后，在步骤 # 1 下，定位：

```
# if you are using reputation preprocessor set these
var WHITE_LIST_PATH ../rules
var BLACK_LIST_PATH ../rules
```

如 Figure 14.17 所示，在两个变量前插入井号（#）。你将不会发布任何站点白名单或黑名单。通过包含井号（#），你告诉 Snort 忽略整行；这通常被称为“注释掉该行”。

14. 在步骤 # 2 的最后，定位：

```
# config logdir:
```

删除井号（#）并在行尾插入 c:\snort\log，如 Figure 14.18 所示。删除井号（#）并添加路径可确认日志目录的创建，这对于审计日志是必需的。

![](img/2459befd842d6fed299fabe6da3512d0_387_0.png)

FIGURE 14.17 Whitelist and Blacklist Changed

![](img/2459befd842d6fed299fabe6da3512d0_387_1.png)

FIGURE 14.18 Configuring Log Directory

15. 跳过步骤 # 3 并继续步骤 # 4。定位：

```
# path to dynamic preprocessor libraries
dynamicpreprocessor directory
/usr/local/lib/snort_dynamicpreprocessor/
```

替换以下内容：

/usr/local/lib/snort_dynamicpreprocessor/ 替换为以下内容：
C:\Snort\lib\snort_dynamicpreprocessor

如 Figure 14.19 所示，这定义了定位 snort_dynamicpreprocessor 所需的路径。

![](img/2459befd842d6fed299fabe6da3512d0_388_0.png)

# FIGURE 14.19 Dynamic Preprocessor Path

16. 定位以下内容：

```
# path to base preprocessor engine
dynamicengine /usr/local/lib/snort_dynamicengine/libsf_engine.so
```

替换以下内容：

/usr/local/lib/snort_dynamicengine/libsf_engine.so

替换为以下内容： C:\Snort\lib\snort_dynamicengine\sf_engine.dll

如 Figure 14.20 所示，这定义了 snort_dynamicengine 文件的路径，并在 Windows 中更改了所使用的文件名（与 Linux 相反）。

# FIGURE 14.20 Dynamic Engine Path Changed

![](img/2459befd842d6fed299fabe6da3512d0_389_0.png)

17. 定位以下内容：

```
# path to dynamic rules libraries
Dynamicdetection directory /usr/local/lib/snort_dynamicrules
```

如 Figure 14.21 所示，在底部行前插入井号（#）。在行前插入井号（#）可移除 snort_dynamicrules 的使用。

![](img/2459befd842d6fed299fabe6da3512d0_389_1.png)

# FIGURE 14.21 Dynamic Rules Excluded

18. 在步骤 # 5 中，定位以下内容：

```
# Does nothing in IDS mode
preprocessor normalize_ip4
preprocessor normalize_tcp: block, rsv, pad, urp, req_urg,
req_pay, req_urp, ips, ecn, stream
preprocessor normalize_icmp4
preprocessor normalize_ip6
preprocessor normalize_icmp6
```

如 Figure 14.22 所示，在每个 preprocessor 前插入井号（#）。Windows 在本节的预处理器功能上存在问题，因此在每行前插入井号（#）可将其从等式中移除。

![](img/2459befd842d6fed299fabe6da3512d0_390_0.png)

FIGURE 14.22 Inline Packet Normalization Excluded

19. 在步骤 # 5 中，定位以下内容：

```
README.sfportscan
# preprocessor sfportscan: proto { all } memcap { 1000000
} sense_level { low }
```

删除井号（#）以启用端口扫描检测，如 Figure 14.23 所示。

![](img/2459befd842d6fed299fabe6da3512d0_391_0.png)

# **FIGURE 14.23** Enable Portscan

+   20. 在步骤 # 5 底部，定位以下内容：
+   
+   ```text
+   whitelist $WHITE_LIST_PATH/white_list.rules,  \\
+   blacklist $BLACK_LIST_PATH/black_list.rules
+   ```
+   
+   如 Figure 14.24 所示，在每行前插入井号（#）。井号禁用与白名单和黑名单关联的路径。

+   21. 在步骤 # 6 中，定位以下内容：
+   
+   ```text
+   # pcap
+   # output log_tcpdump: tcpdump.log
+   ```
+   
+   如 Figure 14.25 所示，在下方插入 `output alert_fast: alerts.ids`。插入此行将在入侵检测模式下报警时创建日志。

+   22. 你需要创建此文件。在不关闭 `snort.conf` 文件的情况下，点击开始图标并在搜索栏中输入 Word。Microsoft Word 或 WordPad 将出现。按下 Enter 键。

+   23. 选择 文件 > 另存为。你需要导航到 `C: > Snort > Log`。输入 `alert.ids` 作为名称并选择 保存。如果出现警告提示，选择 是。

![](img/2459befd842d6fed299fabe6da3512d0_392_0.png)

# FIGURE 14.24 Whitelist and Blacklist Excluded

![](img/2459befd842d6fed299fabe6da3512d0_392_1.png)

# FIGURE 14.25 Output Alert

+   24. 关闭 `alert.ids` 文档。返回到 `snort.conf` 文件。
+   25. 在步骤 # 7 中，在 `# site specific rules` 下，插入以下内容：

```
Include $RULE_PATH\community.rules.
```

在步骤 # 7 的每行前放置井号（#）。Figure 14.26 显示了它应有的样子。这将指示 Snort 使用 community.rules 文件，并在比较定义规则时忽略所有其他路径。

![](img/2459befd842d6fed299fabe6da3512d0_393_0.png)

# FIGURE 14.26 Setting Rules

+   26. 在步骤 # 8 中，删除每个 include $PREPROC_RULE_PATH 前的井号（#）。将每个实例中的 / 更改为 \。两个更改均显示在 Figure 14.27 中。这将启用预定义的预处理器规则路径。

+   27. 选择 文件 > 保存。通过点击窗口右上角的红色 X 退出 snort.conf 文件。

大部分配置更改要么启用要么禁用了特定功能。Snort 提供了许多 Windows 不能正确使用的功能。如果不通过配置文件，你将无法运行 Snort。

# Adding the Rules

Snort 需要规则来进行比较。有三种级别的规则：社区版、注册版和订阅版。就本实验而言，你

将加载社区规则。它们可能略显过时，但它们是有用且必要的。

+   1. 前往：https://www.snort.org/downloads 下载 `community-rules.tar.gz`。

+   2. 前往你的下载文件位置并选择 `community-rules.tar` 文件并右键单击。选择 7-Zip → 提取文件 以启动提取窗口，如 Figure 14.28 所示。

![](img/2459befd842d6fed299fabe6da3512d0_394_0.png)

**FIGURE 14.27** Step #8 Changes

![](img/2459befd842d6fed299fabe6da3512d0_394_1.png)

**FIGURE 14.28** Extract Window

3. 点击 确定。会出现一个新文件夹。双击 community-rules.tar 文件夹以查看其内容，如 Figure 14.29 所示。

![](img/2459befd842d6fed299fabe6da3512d0_395_0.png)

FIGURE 14.29 Community-Rules

4. 右键单击新的 community-rules.tar 文件，选择 7-Zip > 提取文件 以启动另一个提取窗口。

5. 点击 确定。会出现另一个文件夹。双击 Community-Rules 文件夹。双击下一个 Community-Rules 文件夹以查看少量项目列表。Figure 14.30 显示了 C: > “你的下载文件夹路径” > community-rules.tar > community-rules > community-rules 的内容。

6. 保持此窗口打开。点击 Windows 图标并选择 文件资源管理器。

7. 在文件资源管理器中，导航到 本地磁盘 (C:) > Snort > Rules。将 Community-Rules 文件夹中下载文件夹中的 community 文件复制并粘贴到 Snort > Rules 文件夹中，如 Figure 14.31 所示。

8. 通过点击窗口右上角的红色 X 关闭所有窗口。

# FIGURE 14.30 Community-Rules Folder

# FIGURE 14.31 Community File Moved to Rules Folder

# Running Snort for Network Intrusion Detection

Snort 可以配置为在三种主要模式下运行，并具有多种复杂选项。三种主要模式包括嗅探、数据包记录和网络入侵检测。了解如何实现这三种模式至少是基本的方法很重要。

+   1. 通过在搜索栏中输入 cmd 打开命令提示符。不要按 Enter。在程序下，右键单击 cmd 并选择 以管理员身份运行。UAC 提示时选择 是。

+   2. 你首先需要更改目录。在命令提示符下，输入 cd C:\Snort\bin 并按 Enter，如 Figure 14.32 所示。

![](img/2459befd842d6fed299fabe6da3512d0_397_0.png)

FIGURE 14.32 cd c:\snort\bin

+   3. 第一个命令将是发现机器上的接口。输入 snort -W 并按 Enter。接口列表将出现，如 Figure 14.33 所示。

