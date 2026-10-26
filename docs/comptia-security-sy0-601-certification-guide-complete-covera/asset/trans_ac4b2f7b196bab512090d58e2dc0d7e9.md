
- **Netstat**：Netstat 是 Windows 操作系统中内置的原生工具，用于查看已建立的连接和正在监听的端口。它还可以让你了解计算机上运行了哪些服务。如果重新启动计算机，所有已建立的端口都会消失。请参考以下截图：

![](img/adfe46a7d545f23d50e4108d31e60de2_230_0.png)

图 7.27 – Netstat

> **考试提示**
>
> Netstat 显示已建立和正在监听的端口，但如果重新启动计算机，已建立的连接会消失。

- **Nslookup**：Nslookup 是一种诊断工具，用于验证 DNS 服务器数据库中主机名的 IP 地址。我们还可以使用 `set type=MX` 命令，这会返回域内所有邮件服务器的 DNS 详细信息。请参见以下截图：

- **地址解析协议（ARP）**：ARP 用于将 IP 地址转换为 MAC 地址；`arp -a` 命令显示 ARP 缓存。攻击者可以使用 ARPSpoof、ARPoison 和 Ettercap 来毒化你的 ARP 缓存。这些工具可通过发送未经请求的 ARP 回复来创建 ARP 广播。请阅读 https://openmaniak.com/ettercap_arp.php 上的文章。我们可以通过使用 `arp -s` 向 ARP 缓存添加静态条目来防止 ARP 毒化，或者可以使用 Wireshark 捕获网络中的数据包来检测它：

![](img/adfe46a7d545f23d50e4108d31e60de2_232_0.png)

图 7.30 – ARP 缓存

- **ipconfig/ip/ifconfig**：这些命令显示 IP 配置。Windows 版本是 `ipconfig`，而 Unix/Linux 可以使用 `ifconfig`。

以下截图运行了 `ipconfig /displaydns` 命令，它显示计算机上的 DNS 缓存：

图 7.31 – DNS 缓存

- **ipconfig /flushdns**：此命令用于清除 DNS 缓存中的所有条目。请参见以下截图：

![](img/adfe46a7d545f23d50e4108d31e60de2_233_0.png)

![](img/adfe46a7d545f23d50e4108d31e60de2_233_1.png)

图 7.32 – 清除 DNS 缓存

- **tcpdump**：Linux/Unix 将其用作数据包嗅探命令。

`tcpdump -i eth0` 显示第一个以太网适配器的信息，如以下截图所示：

```text
# tcpdump -i eth0
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
11:33:31.976358 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 3500440357:3500440545, ack 3657268334, win 18760, length 196
11:33:31.976603 IP 172.16.25.125.apwi-rxspooler > 172.16.25.126.ssh: Flags [.], ack 196, win 64487, length 0
11:33:31.977243 ARP, Request who-has tecmint.com tell 172.16.25.126, length 28
11:33:31.977359 ARP, Reply tecmint.com is-at 00:14:5e:67:26:1d (oui Unknown), length 46
11:33:31.977367 IP 172.16.25.126.54807 > tecmint.com: 4240+ PTR? 125.25.16.172.in-addr.arpa. (44)
11:33:31.977599 IP tecmint.com > 172.16.25.126.54807: 4240 NXDomain 0/1/0 (121)
11:33:31.977742 IP 172.16.25.126.44519 > tecmint.com: 40988+ PTR? 126.25.16.172.in-addr.arpa. (44)
11:33:32.028747 IP 172.16.20.33.netbios-ns > 172.16.31.255.netbios-ns: NBT UDP PACKET(137): QUERY; REQUEST; BROADCAST
11:33:32.112045 IP 172.16.21.153.netbios-ns > 172.16.31.255.netbios-ns: NBT UDP PACKET(137): QUERY; REQUEST; BROADCAST
11:33:32.115606 IP 172.16.21.144.netbios-ns > 172.16.31.255.netbios-ns: NBT UDP PACKET(137): QUERY; REQUEST; BROADCAST
```

- **Nmap**：Nmap 是一款免费开源的网络映射器，可用于创建网络上所有设备的清单，也可用于横幅抓取。

- **Netcat**：Netcat（或 nc）是一个用于在 Linux/Unix 环境中显示网络连接的工具。在以下截图中，正在使用 `netcat -z` 命令扫描 78–80 端口，从这里可以看到端口 78 和 79 已关闭，但 HTTP 使用的端口 80 是打开的。`-v` 选项表示详细模式，会显示所有信息。请参考以下截图：

```text
$ netcat -z -v ianneil501.com 78-80

nc: connect to ianneil501.com port 78 (tcp) failed: connection refused
nc: connect to ianneil501.com port 79 (tcp) failed: connection refused

Connection to ianneil501.com port 80 (tcp/html) succeeded!
```

- **IP 扫描器**：以下截图显示了 Angry IP Scanner，它可以扫描给定范围内的所有 IP 地址。它会显示活动且响应的 IP 地址、活动但不响应的 IP 地址，以及不活动的 IP 地址。攻击者可能会使用一个不活动的 IP 地址来访问你的网络，因为他们知道该地址没有分配给任何人。

![](img/adfe46a7d545f23d50e4108d31e60de2_235_0.png)

图 7.35 – Angry IP

- **Curl**：这是一个用于传输数据的命令行工具。它也可以用于横幅抓取；这会从 Web 服务器获取远程横幅信息。`-s` 表示静默模式，`-I` 获取 HTTP 头。

**示例**：`curl -s -I 192.168.24.21` 将获取 HTTP 头，输出可能如下所示：

```text
HTTP/1.1 200 OK
Date: Fri, 16 Oct 2020 17:30:12 GMT
Server: Apache/2.2.8 (Ubuntu) DAV/2
X-Powered-By: PHP/5.2.4-2ubuntu5.24
Content-Type: text/html
```

![](img/adfe46a7d545f23d50e4108d31e60de2_237_0.png)

图 7.36 – HIDS 输出

- **防病毒/高级恶意软件工具**：有不少防病毒/反恶意软件工具会定期扫描计算机并生成报告。以下截图显示了已被隔离的病毒列表。AVG 免费防病毒软件已隔离了这四种病毒，使其无法造成任何损害：

![](img/adfe46a7d545f23d50e4108d31e60de2_237_1.png)

图 7.37 – 已隔离的病毒

- **文件完整性检查器**：微软有一个**系统文件检查器（SFC）**，它可以替换损坏的文件，用 system32 中压缩文件夹内的副本进行替换。您可以使用 `sfc /scannow` 命令运行它，如以下截图所示：

![](img/adfe46a7d545f23d50e4108d31e60de2_238_0.png)

图 7.38 – 系统文件检查器

- **文件校验和完整性验证器（FCIV）**：这是微软的一个实用工具，可以检查系统文件的哈希值，以确保没有被人篡改。

- **文件处理**：在 Linux 环境中，你可能希望更深入地查看不同的文件，包括生成的日志文件。让我们看看管理员可以使用的一些手动工具，从 Cat 开始：

  - a. **连接（cat）**：Linux 中的 `cat` 命令可用于创建文件、查看文件，还可以将多个文件合并到另一个文件中。要创建一个名为 `rugbyfile` 的新文件，我们使用以下语法：

    ```text
    cat > rugbyfile
    ```

    您还可以使用以下语法将三个文件的内容合并到一个输出文件中：

    ```text
    cat file1.txt file2.txt file3.txt | sort > samplefile.txt
    ```

  - b. **Head**：Linux 中最重要的文件是 `/var/log/messages` 文件，它显示系统事件，如关机、重启。我们可以使用 head 命令检查该日志中的前 10 条消息，语法如下：

    ```text
    head /var/log/messages -n 10
    ```

**示例：** 公司里有人没有认真工作，一直在浏览网页，经理让你以安全管理员的身份收集证据。你认为协议分析器或数据包嗅探器是跟踪信息的最佳工具。你运行一次 Wireshark 会话，并捕获对 NFL 网站的访问。分析跟踪记录时，你注意到请求使用的是 HTTP GET 动词。这是对 www.nfl.com 上某个页面的请求。在以下截图中，我们查看的是一篇题为*乔什·多布斯和迈克·格伦农吸引交易兴趣*的文章。URL 是 `https://www.nfl.com/news/josh-dobbs-mike-glennon-drawing-trade-interest-0ap3000000952209`：

![](img/adfe46a7d545f23d50e4108d31e60de2_240_0.png)

图 7.39 – 协议分析器

在 CompTIA Security+ 考试中，协议分析器也可以被称为*数据包嗅探器*。考试中还会使用商业名称，如 Wireshark 和 Tcpdump。

- **tcpreplay**：这是一个开源工具，可用于分析由 Wireshark 或 tcpdump 生成的 `.pcap` 文件，然后重放流量并将其发送到 NIPS，以查看它是否能检测到。

