
```
$ aws ec2 get-console-output --instance-id i-0c87223xxxxx --output text
i-0c87223xxxxx     [    0.000000] Linux version 5.8.0-1041-aws             ❶
(buildd@lcy01-amd64-017) (gcc (Ubuntu 10.3.0-1ubuntu1~20.10) 10.3.0,
 GNU ld (GNU Binutils for Ubuntu) 2.35.1)
 #43-Ubuntu SMP Wed Jul 14 14:49:43 UTC 2021 (Ubuntu 5.8.0-1041.43-aws 5.8.18)
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.8.0-1041-aws
  root=PARTUUID=fd4c3fdd-01 ro console=tty1 console=ttyS0
  nvme_core.io_timeout=4294967295 panic=-1
[    0.000000] KERNEL supported cpus:
...
Welcome to Ubuntu 20.10!
[    8.645187] systemd[1]: Set hostname to <ubuntu>.                       ❷
[    8.650813] systemd[1]: Initializing machine ID from KVM UUID.
[    8.655158] systemd[1]: Installed transient /etc/machine-id file.
...
[  OK  ] Finished Execute cloud user/final scripts.
[  OK  ] Reached target Cloud-init target.
```

❶ 关于虚拟机启动过程的详细信息，包括支持的硬件

❷ 在这里，systemd 启动并接管虚拟机的初始化。

在基础设施或平台运维角色中，你会发现虚拟机级别的信号很有用，无论是用于故障排除还是规划。开发人员可能无法始终访问此空间中的源代码，`JVM` 是一个潜在的例外。在基础知识讲完之后，让我们首先将注意力转向容器化环境。

### 3.2.2 容器

*容器* 是提供应用级依赖管理的便捷抽象。也就是说，容器利用 Linux 内核特性，包括用于隔离的命名空间（例如，进程、网络栈或文件系统挂载点）和用于资源使用管理的 cgroups。容器将这些内核特性封装在简单易用的 API 中，使你能够在一定的隔离保证下启动应用程序。它们非常适合将你的应用程序所需的所有依赖项（从编程语言库到操作系统级资源再到环境变量）打包成一个单一的构件，称为 *container image*，并在主机上运行。对于容器主机，针对虚拟机或裸金属实例的一般观察同样适用——也就是说，通常依赖于 `journald` 来处理事件，而 `journalctl` 是其主要接口。

具体来说，对于容器，信号可以来自容器运行时，例如 `Docker` 或 `containerd` 本身。另一方面，信号也来自正在运行的容器——也就是应用级信号。

对于日志，不同的容器运行时提供不同程度的支持：

* Docker 守护进程（[https://docs.docker.com/config/daemon/#read-the-logs](https://docs.docker.com/config/daemon/#read-the-logs)）提供了一个完整的解决方案，取决于主机环境，通常通过 `journalctl` 或某个文件系统位置，例如 `/var/log/` 下使日志可用。

* 对于 `containerd`，假设使用它的环境（例如，`Kubernetes`）会负责日志管理。换句话说，它不支持自己的端到端解决方案；然而，如果你以 `systemd` 单元运行 `containerd`，`journald` 将捕获输出，为你提供一些基本信息。

* `Kubernetes` 专用的容器运行时接口（`CRI`；[http://mng.bz/0KAJ](http://mng.bz/0KAJ)）是平台无关的，并且不直接支持日志记录。相反，`CRI` 将具体的日志支持推迟给你使用的容器运行时，例如 `containerd`（见上文）。

关于来自应用层的容器日志，我们需要稍微退一步。十二要素应用认为将日志（[https://12factor.net/logs](https://12factor.net/logs)）视为事件流，鼓励应用程序记录到 `stdout`，并且不关心日志路由。这种“记录到 stdout”的做法在容器中也很常见；`Docker` 对容器日志有完整支持（[http://mng.bz/Ke8X](http://mng.bz/Ke8X)），使你能够通过日志驱动程序，例如 `journald`、`syslog` 和 `Fluentd`，以及云提供商特定的格式来管理日志输入和输出。例如，要将 `Docker` 容器的日志直接发送到 Amazon CloudWatch，你会这样配置日志驱动程序：

```
docker run --log-driver=awslogs \
           --log-opt awslogs-region=eu-west-1 \
           --log-opt awslogs-group=docker-test \
           --log-opt awslogs-create-group=true \
       ...
```

如果你想了解更多关于如何在容器中使用日志的信息，我推荐阅读 Phil Wilkins 所著的 *Logging in Action*（Manning，2022；[https://www.manning.com/books/logging-in-action](https://www.manning.com/books/logging-in-action)，该书对此主题进行了详细阐述。

对于指标，容器有一些可用的选项：

* 一个相当流行且广泛使用的暴露指标的解决方案是 `cAdvisor`（[http://mng.bz/9D4j](http://mng.bz/9D4j)），它与 `Prometheus` 一起使用。

* `Telegraf`（[http://mng.bz/WzRg](http://mng.bz/WzRg)）支持一个 `Docker` 插件（[https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker)）可生成丰富的指标，包括容器统计、内存、CPU 使用率和网络相关指标。

* `Docker` 提供了一个原生的（[https://docs.docker.com/config/daemon/prometheus/](https://docs.docker.com/config/daemon/prometheus/)）`Prometheus` 集成，其范围仅限于运行时（截至撰写时，不涵盖应用程序）。

具体来说，如果你使用的是 `Docker` 守护进程（[https://docs.docker.com/config/daemon/](https://docs.docker.com/config/daemon/)），你可以通过其套接字获得（低级）指标访问权限，如以下代码所示。`curl` 命令的输出已被编辑以显示一些示例统计信息；它采用 JSON 格式，因此你可以直接查询，例如使用 `jq`：

```
$ sudo docker run -d --rm  nginx                                         ❶
82cea007263254659534e47cfb13b951f3426f6d91a16246143480f18a217718
 
$ sudo curl --unix-socket /var/run/docker.sock \
    "http://localhost/v1.40/containers/82cea0072632/stats?stream=false"  ❷
 
{
 ... "system_cpu_usage":18084207160000000,"online_cpus":2,
 ... "rss":1892352,"rss_huge":0,"total_active_anon":1892352,
 ... "total_active_file":0,"total_cache":0,"total_dirty":0,
}
```

❶ 启动一个长期运行的容器——在本例中为 NGINX 服务器。请注意此处的容器 ID `82cea007263254659534e47cfb13b951f3426f6d91a16246143480f18a217718`，我们在下一步查询指标时需要它。

❷ 使用 `curl` 查询 UNIX 套接字 `/var/run/docker.sock`。`Docker` 守护进程提供启动或停止容器等服务。我们需要提供我们想要获取指标的容器 ID，或者至少提供唯一标识容器的部分（在本例中为 `82cea0072632`）。

表 3.2 显示了一些示例指标（来自 `cAdvisor`），你通常可以从容器及其运行时中找到这些指标。

表 3.2 容器指标示例

| 类别 || cAdvisor 中的示例指标 |

| --- | --- |

| CPU || `container_cpu_usage_seconds_total`（累计 CPU 时间消耗）和 `container_cpu_load_average_10s`（过去 10 秒内容器的 CPU 负载平均值） |

| --- | --- |

| 内存 || `container_memory_max_usage_bytes`（最大应用内存使用量）和 `machine_memory_bytes`（主机上安装的内存） |

| 文件系统 || `container_file_descriptors`（容器的打开文件描述符） |

| 网络 || `container_network_receive_packets_total`（接收数据包的累计计数） |

