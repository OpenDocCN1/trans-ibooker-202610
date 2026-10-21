

## system directive（设置系统范围配置）

你可以使用 system directive 设置各种系统范围的配置。system directive 中可用的配置选项包括：

- log_level
- suppress_repeated_stacktrace
- emit_error_log_interval
- suppress_config_dump
- without_source
- process_name

你可以使用如下方式使用系统配置：

```
<system>
  process_name my_app
  log_level error
  without_source
</system>
```

这里我们首先将 supervisor 和 worker 进程名称设置为 my_app，然后将默认日志级别设置为 error。我们还指示 Fluentd 在没有任何输入插件的情况下启动。

## label directive（用于输出分组和路由）

标签主要通过允许路由部分不遵循配置文件中自上而下的解析顺序来解决维护和管理配置文件的问题。你也可以将标签视为 go-to 语句。以下是使用标签的简单配置示例：

```
<source>
  @type http
  bind 0.0.0.0
port 9999
@label @MYCLUE
</source>

<filter test.session>
    @type grep
    <exclude>
        key action
        pattern ^login$
    </exclude>
</filter>

<label @MYCLUE>
    <filter test.session>
        @type grep
        <exclude>
            key action
            pattern ^logout$
        </exclude>
    </filter>

    <match test.session>
        @type stdout
    </match>
</label>
```

在前面的配置中，source 下的 @label 参数是向 @MYCLUE 标签部分的重定向。控制流将跳过 login 的 filter 定义，并直接移动到 @MYCLUE 标签。

## @include directive（导入配置文件）

使用 @include directive，你可以从单独的目录导入配置文件。@include directive 支持如下的常规文件路径、glob 模式和 HTTP URL 规范：

```
# absolute path
@include /path/to/config.conf

# if using a relative path, the directive will use
# the dirname of this config file to expand the path
@include extra.conf

# glob match pattern
@include config.d/*.conf

# http
@include http://example.com/fluent.conf
```

在继续之前，让我们快速了解一下如何运行 agent 来查看日志。要运行如下的简单配置：

```
<source>
  @type http
  port 9999
  bind 0.0.0.0
</source>

<filter myapp.test>
  @type grep
  <exclude>
    key action
    pattern ^logout$
  </exclude>
</filter>

<match myapp.test>
  @type stdout
</match>
```

你需要将前面的配置替换到默认位置 /etc/td-agent/td-agent.conf。你现在可以在 macOS 上按如下方式重新加载 td-agent：

```
$ sudo launchctl load /Library/LaunchDaemons/td-agent.plist
#check logs
$ tail -f /var/log/td-agent/td-agent.log
2020-12-21 12:21:57 +0530 [info]: parsing config file is succeeded path="/etc/td-agent/td-agent.conf"
2020-12-21 12:21:58 +0530 [info]: using configuration file:
<ROOT>
  <source>
    @type http
    port 9999
    bind "0.0.0.0"
  </source>
  <filter myapp.test>
    <exclude>
        key "action"
        pattern ^logout$
    </exclude>
</filter>
<match myapp.test>
    @type stdout
</match>
</ROOT>
2020-12-21 12:21:58 +0530 [info]: starting fluentd-1.0.2
pid=43215 ruby="2.4.2"
2020-12-21 12:21:58 +0530 [info]: spawn command to main:
cmdline=["/opt/td-agent/embedded/bin/ruby", "-Eascii-8bit:ascii-8bit", "/opt/td-agent/usr/sbin/td-agent", "--log", "/var/log/td-agent/td-agent.log", "--use-v1-config", "--under-supervisor"]
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-elasticsearch' version '2.4.0'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-kafka' version '0.6.5'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-rewrite-tag-filter' version '2.0.1'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-s3' version '1.1.0'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-td' version '1.0.0'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-td-monitoring' version '0.2.3'
2020-12-21 12:21:58 +0530 [info]: gem 'fluent-plugin-webhdfs' version '1.2.2'
2020-12-21 12:21:58 +0530 [info]: gem 'fluentd' version '1.0.2'
2020-12-21 12:21:58 +0530 [info]: adding filter
pattern="myapp.test" type="grep"
2020-12-21 12:21:58 +0530 [info]: adding match
pattern="myapp.test" type="stdout"
2020-12-21 12:21:58 +0530 [info]: adding source type="http"
2020-12-21 12:21:58 +0530 [info]: #0 starting fluentd worker
pid=43221 ppid=43215 worker=0
2020-12-21 12:21:58 +0530 [info]: #0 fluentd worker is now running worker=0
```

现在你可以运行以下 curl 命令来模拟不同的操作：

```
$ curl -i -X POST -d 'json={"action":"login","user":2}' http://localhost:9999/myapp.test
```

```
HTTP/1.1 200 OK
Content-Type: text/plain
Connection: Keep-Alive
Content-Length: 0
```

由于我们当前传递的 action 为 `login`，我们可以检查日志位置以验证它们是否已打印：

```
2020-12-21 12:31:31.529967000 +0530 myapp.test:
{"action":"login","user":2}
```

如果你尝试在以下 curl 命令中发布 action 为 `logout`，它将根据我们指定的 filter 规则被丢弃，你将无法看到任何日志被打印：

```
$ curl -i -X POST -d 'json={"action":"logout","user":2}' http://localhost:9999/myapp.test
HTTP/1.1 200 OK
Content-Type: text/plain
Connection: Keep-Alive
Content-Length: 0
```

现在你了解了 Fluentd 的工作原理，让我们来研究如何在云原生环境中集成它并在像 Kubernetes 这样的容器编排器上部署它。

## Fluentd on Kubernetes

默认情况下，在 Kubernetes 中，容器中的日志会写入标准输出和标准错误流。这是 Kubernetes 开箱即用的基本 I/O 日志记录。容器引擎将流重定向到 Kubernetes 中配置的日志驱动程序，以将日志以 JSON 格式写入文件。作为用户，如果你想查看日志，可以使用 `kubectl logs` 命令。然而，这种方法的问题非常明显：如果容器崩溃，或者 Pod 被驱逐，或者节点本身死亡，你将无法再访问应用程序日志。因此，日志应该具有独立于容器、Pod 和节点的存储。这种概念正式被称为 *cluster-level logging*，它需要一个单独的后端来存储、分析和查询日志。虽然 Kubernetes 本身并不提供集群级日志记录的解决方案，但我们可以将 Fluentd 与 Elasticsearch 等存储解决方案结合起来构建一个可扩展的解决方案。

在继续介绍在 Kubernetes 上部署 Fluentd 的首选方式之前，让我们先看看集群级日志记录可以通过哪些不同方法实现，以及它们的优缺点。基本上构建集群级日志记录解决方案有两种方式：

- 在每个应用程序 Pod 中使用一个 sidecar 容器。
- 使用在每个节点上运行的节点级日志代理。

让我们更详细地了解一下这两种解决方案。

## Sidecar container approach

通常，在 Kubernetes 中你会在 Pod 中运行应用程序，如第 5 章所述。在 sidecar 方法中，你可以在应用程序 Pod 内创建一个或多个 sidecar 容器。sidecar 容器方法可以用两种不同的方式使用：

- 你可以使用 sidecar 容器将应用程序日志流式传输到容器的自己的 stdout。
- 你可以在 sidecar 容器中运行一个日志代理，该代理基本上从应用程序容器中获取日志并将其发送到日志后端。

sidecar 容器方法有一些优点：

- 你可以通过拥有两个 sidecar 容器来流式传输特定日志文件到不同的日志流，从而将以不同日志格式产生的应用程序日志发送出去（结构化或非结构化）。
- Sidecar 容器可以从文件、套接字或 journald 读取日志，并且每个 sidecar 容器会将日志打印到 stdout 和 stderr

