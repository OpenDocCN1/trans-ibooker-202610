

5. 在审计系统上的网络服务时，使用 lsof 实用程序和 fuser 实用程序的主要区别是什么？

A. lsof 命令实用程序显示系统上的打开文件。
B. fuser 命令实用程序显示正在使用的端口和协议。
C. lsof 命令实用程序已弃用。
D. fuser 命令实用程序显示使用端口的进程 PID。
E. lsof 命令实用程序显示正在使用的端口和协议。

6. Hiro 已完成对其 systemd 系统的网络服务审计。他现在需要禁用所有不必要的网络服务。他应该使用什么命令？

A. service
B. systemctl
C. chkconfig
D. update-rc.d
E. init

7. Hiro 发现他正在审计的当前 systemd 系统上有一个超级服务器。他确定 xinetd 管理着一些不需要的网络服务。他应该如何禁用这些服务？（可多选。）

A. 使用 systemctl 命令将 xinetd 服务设置为 disabled，以便其不会在启动时启动。
B. 将服务的 disable 指令设置为 yes 在 /etc/xinetd.conf 文件中。
C. 使用 chkconfig 命令在所有运行级别上禁用 xinetd，以便其不会在启动时启动。
D. 将服务的 disable 指令设置为 yes 在其 /etc/xinetd.d/ 目录文件中。
E. 卸载 xinetd 服务。

8. Marcus 正在管理一台采用 TCP Wrappers 的系统。他确定 TCP Wrapper 访问文件的限制不够严格。他可以采取什么措施来改进其安全性？

A. 在 /etc/hosts.allow 文件中放入 ALL: ALL。
B. 在 /etc/host.allow 文件中放入 ALL: ALL。
C. 在 /etc/hosts.deny 文件中放入 ALL: ALL。
D. 在 /etc/host.deny 文件中放入 ALL: ALL。
E. 在 /etc/host.deny 文件和 /etc/host.allow 文件中放入 ALL: ALL。

9. 关于 Linux 中的密码，以下哪些说法是正确的？（可多选。）

A. 密码应存储在 /etc/shadow 文件中。
B. 密码应存储在 /etc/passwd 文件中。
C. 密码存储为带盐的哈希值。
D. 使用 pwconv 实用程序将密码移动到 /etc/shadow 文件中。
E. 使用 passwd 命令更改您的密码。

10. Yoyo 是维珍银河的新系统管理员。她需要使用超级用户权限来执行几项职责。应该怎么做？

A. 将由系统管理团队共享的 root 账户密码给 Yoyo，并让她通过 GUI 登录到 root 账户。
B. 将由系统管理团队共享的 root 账户密码给 Yoyo，并让她通过 su - 命令登录到 root 账户。
C. 将由系统管理团队共享的 root 账户密码给 Yoyo，并让她通过 su -c 命令执行需要超级用户权限的命令。
D. 在 /etc/sudoers 文件中设置 Yoyo，并让她通过 sudo 命令执行需要超级用户权限的命令。
E. 在 /etc/sudoers 文件中设置 Yoyo，并让她通过 su -c 命令执行需要超级用户权限的命令。

11. 哪个命令提供关于当前登录系统的用户的最多信息？

A. who
B. w
C. last
D. who am i
E. whoami

12. Wade 需要设置他的游戏服务器上每个虚拟现实游戏玩家可以启动的进程数量。他应该在他们的环境文件中使用哪个 ulimit 选项？

A. -a
B. -l
C. -t
D. -u
E. -v

13. OpenSSH 应用程序在哪个文件中跟踪之前连接过的主机及其公钥？

A. ~/.ssh/known_hosts
B. ~/.ssh/authorized_keys
C. /etc/ssh/known_hosts
D. /etc/ssh/authorized_keys
E. /etc/ssh/ssh_host_rsa_key.pub

14. 以下哪些是 OpenSSH 配置文件？（可多选。）

A. ~/.ssh/config
B. /etc/ssh/ssh_config
C. /etc/ssh/sshd_config
D. /etc/sshd/ssh_config
E. /etc/sshd/sshd_config

15. 以下哪些文件可能涉及使用 SSH 密钥进行身份验证？

A. /etc/ssh/ssh_host_rsa_key
B. /etc/ssh/ssh_host_rsa_key.pub
C. ~/.ssh/id_rsa_key
D. ~/.ssh/id_rsa_key.pub
E. ~/.ssh/id_rsa

16. Wade 的 OpenSSH 私钥已被泄露，所以他需要为自己创建一个新的公钥/私钥对。使用超级用户权限，他应该使用什么命令？

A. ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
B. ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key
C. ssh-keygen -t rsa -f ~/.ssh/id_rsa_key
D. ssh-keygen -t rsa -f ~/.ssh/id_ecdsa_key
E. ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key.pub

17. DeAndre 希望在使用 OpenSSH 连接到远程系统时使用无密码身份验证。他已生成所需的公钥/私钥对，并准备将公钥复制到 ~/.ssh/authorized_keys 文件中存储。他应该使用哪个实用程序来安全地执行此操作？

A. scp
B. ssh-keygen
C. scp-id-copy
D. scp-copy-id
E. ssh-copy-id

18. Aleena 已生成她的公钥/私钥对，并将公钥放在了相应的位置。她现在想使用 OpenSSH 代理会话，以便不必多次输入密码。她应该输入的下一个命令是什么？

A. gpg-agent
B. ssh-copy-id
C. ssh-add
D. ssh-agent /bin/bash
E. ssh

19. 系统管理员 Reagan 最近使用 gpg --import zer0es.pub 命令导入了她朋友 Aleena 的公钥。该密钥现在在哪里？（可多选。）

A. 在 Reagan 的密钥环上
B. 在 zer0es.pub 文件中
C. 在 gpg-agent 中
D. 在 reagan.pub 文件中
E. 在 ~/.gnupg/ 文件中

20. Wade 想要创建数字签名。他可以使用哪个 gpg 选项来执行此操作？（可多选。）

A. --sign
B. --clearsign
C. --verify
D. --detach-sig
E. --out

## 复习问题答案

![](img/ca7617340ddbe2360d36ce3428729fa9_634_0.png)

## 第 1 章：探索 Linux 命令行工具

1. D. Linux 上的 /bin/sh 文件现在通常指向 shell 程序，例如 /bin/bash。因此，选项 D 是正确答案。其他选项都是 shell 程序。因此，选项 A、B、C 和 E 是错误的选择。

2. C. uname -r 命令将仅显示当前 Linux 内核版本（修订版）。因此，选项 C 是正确答案。uname 命令将仅显示当前操作系统名称 Linux，因此选项 A 是错误答案。echo $BASH_VERSION 命令将显示 Bash shell 的当前版本。因此，选项 B 也是错误答案。uname -a 命令将显示当前 Linux 内核版本以及其他附加信息，因此选项 D 是错误选择。echo $SHELL 命令显示当前使用的 shell 程序。因此，选项 E 也是错误选择。

3. D. echo \^New \^Style 命令将显示 ^New ^Style，因为反斜杠是一种 shell 引用形式，可保护其后的单个字符。因此插入符号 (^) 受到保护并在输出中每个单词之前显示。因此，选项 D 正确。选项 A、B、C 和 E 是错误答案，因为这些命令不会显示这些输出。

4. A、C、D。which fortytwo.sh 命令将搜索 $PATH 环境变量中列出的目录以查找 fortytwo.sh 程序。如果找到，它将显示程序的绝对目录引用。因此，选项 A 是正确答案（也是最佳答案）。如果您已经知道 fortytwo.sh 程序的目录位置，则通过 echo $PATH 命令显示 $PATH 变量中存储的目录名称也同样有效。因此，选项 C 也是正确答案。通过发出 fortytwo.sh 命令尝试运行程序也可以确定程序是否存储在 $PATH 目录中，因为如果是，程序将运行。另一方面，如果它不存储在 $PATH 目录中，您将收到 not found 错误消息。因此，选项 D 也是正确答案。通过 cat fortytwo.sh 命令显示文件无法帮助确定程序是否存储在 $PATH 目录中，因此选项 B 是错误答案。从名为 /usr/bin/ 的目录尝试运行程序也无法帮助您做出此判断，因为未声明它位于那里。因此，选项 E 也是错误选择。

5. A、C、D、E。vim 编辑器的三种模式是命令模式（也称为普通模式）、插入模式（也称为编辑或输入模式）和 ex 模式（有时称为冒号命令模式）。因此，选项 A、C、D 和 E 是正确答案。本题唯一错误的选项是选项 B。

6. C。head 命令可以使用 -n 15 开关或 -15 开关来显示文件的前 15 行。因此，选项 C 是正确答案。要显示除最后 15 行之外的所有行，需要使用 -n -15 开关，因此选项 A 不正确。要显示除前 15 行之外的所有行，需要使用 tail 命令，而不是 head

