### 第 4 章、"システム・ハードニング"

#### 1.

以下のコマンドでワーカー・ノードにシェル・インする：

```
$ vagrant ssh kube-worker-1
```

ポート21を公開しているプロセスを特定する。これを行う一つの方法は、`lsof` コマンドを使うことである。ポートを公開するコマンドは`vsftpd` である：

```
$ sudo lsof -i :21
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
vsftpd  10178 root    3u  IPv6  56850      0t0  TCP *:ftp (LISTEN)
```

あるいは、次のように`ss` ：

```
$ sudo ss -at -pn '( dport = :21 or sport = :21 )'
State   Recv-Q   Send-Q   Local Address:Port \
   Peer Address:Port   Process
LISTEN  0   32   *:21 \
   *:*   users:(("vsftpd",pid=10178,fd=3))
```

プロセス`vsftpd` はサービスとして開始された：

```
$ sudo systemctl status vsftpd
● vsftpd.service - vsftpd FTP server
     Loaded: loaded (/lib/systemd/system/vsftpd.service; enabled; \
             vendor preset: enabled)
     Active: active (running) since Thu 2022-10-06 14:39:12 UTC; \
             11min ago
   Main PID: 10178 (vsftpd)
      Tasks: 1 (limit: 1131)
     Memory: 604.0K
     CGroup: /system.slice/vsftpd.service
             └─10178 /usr/sbin/vsftpd /etc/vsftpd.conf

Oct 06 14:39:12 kube-worker-1 systemd[1]: Starting vsftpd FTP server...
Oct 06 14:39:12 kube-worker-1 systemd[1]: Started vsftpd FTP server.
```

サービスを停止し、パッケージをデインストールする：

```
$ sudo systemctl stop vsftpd
$ sudo systemctl disable vsftpd
$ sudo apt purge --auto-remove -y vsftpd
```

ポートを確認すると、もうリストには載っていない：

```
$ sudo lsof -i :21
```

ノードの外に出る：

```
$ exit
```

#### 2.

以下のコマンドでワーカー・ノードにシェル・インする：

```
$ vagrant ssh kube-worker-1
```

コマンド`sudo vim /etc/apparmor.d/network-deny` を使用して、AppArmorプロファイルを`/etc/apparmor.d/network-deny` に作成する。ファイルの内容は以下のようになる：

```
#include <tunables/global>

profile network-deny flags=(attach_disconnected) {
  #include <abstractions/base>

  network,
}
```

以下のコマンドを実行して、AppArmorプロファイルを強制する：

```
$ sudo apparmor_parser /etc/apparmor.d/network-deny
```

AppArmor 用のアノテーションを追加するために、既存のPodオブジェクトを変更することはできない。まずオブジェクトを削除する必要がある。Podの定義をファイルに書き出す：

```
$ kubectl get pod -o yaml > pod.yaml
$ kubectl delete pod network-call
```

`pod.yaml` ファイルを編集し、AppArmorアノテーションを追加する。関連する注釈では、コンテナの名前`network-call` をキー・サフィックスの一部として使用し、`localhost/network-deny` を値として使用する。接尾辞`network-deny` は、AppArmorプロファイルの名前を指す。最終的なコンテンツは、少し整理すると以下のようになる：

```
apiVersion: v1
kind: Pod
metadata:
  name: network-call
  annotations:
    container.apparmor.security.beta.kubernetes.io/network-call: \
    localhost/network-deny
spec:
  containers:
  - name: network-call
    image: alpine/curl:3.14
    command: ["sh", "-c", "while true; do ping -c 1 google.com; \
              sleep 5; done"]
```

マニフェストからPodを作成する。数秒後、Podは「Running」ステータスに移行するはずだ：

```
$ kubectl create -f pod.yaml
$ kubectl get pod network-call
NAME           READY   STATUS    RESTARTS   AGE
network-call   1/1     Running   0          27s
```

AppArmorがPodのネットワーク呼び出しを防止している。ログをチェックして確認することができる：

```
$ kubectl logs network-call
...
sh: ping: Permission denied
sh: sleep: Permission denied
```

ノードの外に出る：

```
$ exit
```

#### 3.

以下のコマンドでワーカー・ノードにシェル・インする：

```
$ vagrant ssh kube-worker-1
```

seccompプロファイルのターゲットディレクトリを作成する：

```
$ sudo mkdir -p /var/lib/kubelet/seccomp/profiles
```

ディレクトリ`/var/lib/kubelet/seccomp/profiles` に、以下の内容のファイル`audit.json` を追加する：

```
{
    "defaultAction": "SCMP_ACT_LOG"
}
```

セキュリティコンテキスト経由でseccomp 設定を追加するために、既存のPodオブジェクトを修正することはできない。まずオブジェクトを削除する必要がある。Podの定義をファイルに書き出す：

```
$ kubectl get pod -o yaml > pod.yaml
$ kubectl delete pod network-call
```

`pod.yaml` ファイルを編集する。seccompのプロファイルを定義に点す。最終的には以下のようになる：

```
apiVersion: v1
kind: Pod
metadata:
  name: network-call
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
  containers:
  - name: network-call
    image: alpine/curl:3.14
    command: ["sh", "-c", "while true; do ping -c 1 google.com; \
              sleep 5; done"]
    securityContext:
      allowPrivilegeEscalation: false
```

マニフェストからPodを作成する。数秒後、Podは「Running」ステータスに移行するはずだ：

```
$ kubectl create -f pod.yaml
$ kubectl get pod network-call
NAME           READY   STATUS    RESTARTS   AGE
network-call   1/1     Running   0          27s
```

例えば、`sleep` コマンドのようなシステムコールのログエントリを発見できるはずだ：





```
$ sudo cat /var/log/syslog
Oct  6 16:25:06 ubuntu-focal kernel: [ 2114.894122] audit: type=1326 \
audit(1665073506.099:23761): auid=4294967295 uid=0 gid=0 \
ses=4294967295 pid=19226 comm="sleep" exe="/bin/busybox" \
sig=0 arch=c000003e syscall=231 compat=0 ip=0x7fc026adbf0b \
code=0x7ffc0000
```

退出节点：

```
$ exit
```

在文件`pod.yaml`中创建 Pod 定义：

```
apiVersion: v1
kind: Pod
metadata:
  name: sysctl-pod
spec:
  securityContext:
    sysctls:
    - name: net.core.somaxconn
      value: "1024"
    - name: debug.iotrace
      value: "1"
  containers:
  - name: nginx
    image: nginx:1.23.1
```

创建 Pod 并检查状态。可以看到状态为"`SysctlForbidden`"：

```
$ kubectl create -f pod.yaml
$ kubectl get pods
NAME         READY   STATUS            RESTARTS   AGE
sysctl-pod   0/1     SysctlForbidden   0          4s
```

查看事件日志即可知道原因：

```
$ kubectl describe pod sysctl-pod
...
Events:
  Type     Reason           Age    From    \
             Message
  ----     ------           ----   ----    \
             -------
  Warning  SysctlForbidden  2m48s  kubelet \
             forbidden sysctl: "net.core.somaxconn" \
             not allowlisted
```
