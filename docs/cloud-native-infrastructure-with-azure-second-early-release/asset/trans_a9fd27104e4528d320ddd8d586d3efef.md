

不仅这些环境拥有众多活动部件，而且它们也更加复杂，因为现在系统是分布式且临时的，如 Kubernetes Pod。这意味着日志需要被传输出去并集中存储，以便日后查看。

为了解决云原生环境中大规模日志记录的问题，我们需要一种可以轻松部署到环境中并无缝集成的解决方案。这就是 Fluentd 登场的时刻。

## 使用 Fluentd 进行日志记录

Fluentd 是 CNCF 的一个开源项目，可以收集、解析、转换和分析日志。Fluentd 基本上通过将数据结构化为 JSON（见图 6-5）充当统一的日志层。这反过来有助于它在多个来源和目的地之间收集、过滤、缓冲和输出日志。

![](img/8d47d1b5dbca55e784ae602145a894c7_198_0.png)

图 6-5. Fluentd 作为日志层

Fluentd 用 CRuby 编写，CRuby 是 Ruby 编程语言的 C 实现，内存占用小，仅为 30 到 40 MB，并且每个核心每秒处理大约 13K 个事件。让我们来看看 Fluentd 的一些核心概念，以及它如何用于在云原生环境中启用日志记录。

首先，根据你的操作系统发行版，从官方 Fluentd 网站安装代理：https://docs.fluentd.org/installation。

### NOTE

官方稳定的 Fluentd 分发包称为 td-agent。它确保稳定性，并附带了原始作者 Treasure Data Inc 推荐的预配置设置。

下载并为你的发行版安装 td-agent 后，确保它正在运行。例如，在 macOS 上你可以通过 launchctl 来实现：

```
$ ~ sudo launchctl load /Library/LaunchDaemons/td-agent.plist
Password:
$
```

你可以通过以下位置的日志确认代理是否正在运行：

```
$ ~ tail -f /var/log/td-agent/td-agent.log
2020-12-17 22:57:46 +0530 [info]: adding match pattern="td.*.*" type="tdlog"
2020-12-17 22:57:46 +0530 [warn]: #0 [output_td] secondary type should be same with primary one primary="Fluent::Plugin::TreasureDataLogOutput" secondary="Fluent::Plugin::FileOutput"
2020-12-17 22:57:46 +0530 [info]: adding match pattern="debug.**" type="stdout"
2020-12-17 22:57:46 +0530 [info]: adding source type="forward"
2020-12-17 22:57:46 +0530 [info]: adding source type="http"
2020-12-17 22:57:46 +0530 [info]: adding source type="debug_agent"
2020-12-17 22:57:46 +0530 [info]: #0 starting fluentd worker pid=96320 ppid=95841 worker=0
2020-12-17 22:57:47 +0530 [info]: #0 [input_debug_agent] listening dRuby uri="druby://127.0.0.1:24230" object="Fluent::Engine"
2020-12-17 22:57:47 +0530 [info]: #0 [input_forward] listening port port=24224 bind="0.0.0.0"
```

```
2020-12-17 22:57:47 +0530 [info]: #0 fluentd worker is now
running worker=0
```

Fluentd 最好通过其配置文件来理解。你可以在 /etc/td-agent/td-agent.conf 找到 Fluentd 的默认配置文件。配置文件包含许多指令，如以下小节所述。

## source 指令（所有数据的摄入位置）

source 指令确定需要启用的数据输入源（即数据需要从何处摄入）。source 指令将事件提交到 Fluentd 路由系统。默认情况下，Fluentd 提供两个标准输入插件。http 插件提供 HTTP 端点以监听传入的 HTTP 消息；forward 插件提供 TCP 端点以接受 TCP 数据包：

```
<source>
@type forward
port 24224
</source>
```

```
<source>
@type http
port 9999
bind 0.0.0.0
</source>
```

前面的定义指定了两个源：在第一个源中，我们从 TCP 端口 24224 接收事件，在第二个源中，我们有一个 HTTP 服务器监听 TCP 端口 9999。

source 提交给 Fluentd 路由引擎的事件包含三个实体：tag、time 和 record。例如，在以下日志输出中：

```
2020-12-16 15:38:27 +0900 test.root: {"action":"logout","user":3}
```

2020-12-16 15:38:27 +0900 是时间，由输入插件指定，必须采用 Unix 时间格式；test.root 是标签（根据 Fluentd，强烈建议标签采用小写且由数字和下划线混合组成）；而 {"action":"logout","user":3} 是一个 JSON 对象。

输入插件负责从外部源检索事件日志。通常，输入插件会创建一个线程、一个套接字和一个监听套接字。输入插件不仅限于 http 和 forward。以下是一些额外的插件：

### in_syslog

用于通过 Syslog 协议在 UDP 或 TCP 上检索日志事件。Fluentd 在端口 5140 上创建一个套接字；然后，你只需设置你的 syslog 守护进程在该套接字上发送日志事件即可：

```
<source>
  @type syslog
  port 5140
  bind 0.0.0.0
  tag system
</source>
```

### in_tail

允许 Fluentd 读取日志文件的尾部，就像 Unix tail -f 命令一样：

```
<source>
  @type tail
  path /var/log/httpd-access.log
  pos_file /var/log/td-agent/httpd-access.log.pos
```

```
tag apache.access
<parse>
    @type apache2
</parse>
</source>
```

Fluentd 提供额外的输入插件，例如 in_unix 用于从 Unix 域套接字检索记录，以及 in_exec 用于执行外部程序并提取事件日志。你甚至可以编写自己的自定义输入插件。

## match 指令（对输入数据做什么）

match 指令通过查找具有匹配标签的事件来确定输出目的地。Fluentd 的标准输出插件包括 file 和 forward。match 指令必须包含一个匹配模式和一个 @type 参数，该参数定义要使用的输出插件。一旦具有标签的事件与模式匹配，它将被发送到输出目的地。以下是 match 指令的示例用法：

```
<match mywebapp.access>
    @type file
    path /var/log/mywebapp/access
</match>
```

在前面的配置中，我们匹配标记为 mywebapp.access 的事件，并将它们存储在文件 /var/log/mywebapp/access 中。

除了 file 和 forward 之外，你还可以拥有其他类型的输出插件，它们基本上决定了你想要将数据写入的位置。例如，out_elasticsearch 是一个输出插件，将记录写入 Elasticsearch 集群端点。你首先需要使用以下命令安装插件：

```
fluent-gem install fluent-plugin-elasticsearch
```

然后你可以使用以下配置将输出数据发送到 Elasticsearch 端点：

```
<match my.logs>
  @type elasticsearch
  host 192.168.23.13
  port 9200
  logstash_format true
</match>
```

同样，你可以使用 out_copy 将事件复制到多个输出，使用 out_kafka2 将记录发送到 Kafka 集群，使用 out_mongo 将记录写入 MongoDB 数据库，以及使用 stdout 将事件简单地写入标准输出。

## filter 指令（事件处理管道）

filter 指令就像一条规则，根据条件通过或拒绝事件。filter 的语法与 match 相同，但 filter 可以链接到处理管道。你可以按如下方式添加 filter：

```
<source>
  @type http
  port 9999
  bind 0.0.0.0
</source>
```

```
<filter test.session>
  @type grep
  <exclude>
    key action
    pattern ^logout_session$
  </exclude>
</filter>
```

```
<match test.session>
  @type stdout
</match>
```

在前面的配置中，一旦数据被源化，它会经过 filter 部分然后是 match 部分。filter 将根据其类型和规则接受或拒绝事件。在这里我们丢弃了 logout_session 操作，并在 filter 中使用类型 grep 来排除 action 键具有 logout 字符串的任何消息。

以下是一些额外的 filter 插件：

filter_record_transformer

用于更改传入的事件流。你可以使用以下配置示例用于 record_transformer：

```
<filter myweb.access>
    @type record_transformer
    <record>
        host_param "#{Socket.gethostname}"
    </record>
</filter>
```

在前面的配置中，record_transformer 将向日志事件添加 host_param 字段。

filter_geoip

使用 MaxMind GeoIP 数据库向日志添加地理位置信息。

filter_stdout

将事件打印到标准输出，主要用于调试目的。

