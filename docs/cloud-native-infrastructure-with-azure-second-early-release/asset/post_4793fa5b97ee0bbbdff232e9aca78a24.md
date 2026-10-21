
## system directive (sets system-wide configuration)

You can set a variety of system-wide configurations using the system directive. Some of the configuration options available in the system directive are:

- log_level
- suppress_repeated_stacktrace
- emit_error_log_interval
- suppress_config_dump
- without_source
- process_name

You can use the system configuration as follows:

```
<system>
  process_name my_app
  log_level error
  without_source
</system>
```

Here we are first setting the supervisor and worker process names to my_app and then setting the default log level to error. We are also instructing Fluentd to start without any input plug-in.

## label directive (for output grouping and routing)

Labels primarily solve the problem of maintaining and managing the configuration file by allowing routing sections that do not follow the top-to-bottom parsing order in the configuration file. You can think of labels as go-to statements as well. Here is a simple configuration example for using labels:

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

In the preceding configuration, the @label parameter under source is the redirection toward the @MYCLUE label section. The control will skip the filter definition for login and move directly to the @MYCLUE label.

## @include directive (to import config files)

Using the @include directive, you can import configuration files from a separate directory. The @include directive supports regular file path, glob pattern, and HTTP URL conventions as follows:

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

Before moving forward, let’s take a quick look at how you run the agent to see your logs. To run a simple configuration like the following:

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

you would replace the preceding configuration at the default location /etc/td-agent/td-agent.conf. You can now reload the td-agent in macOS as follows:

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
    @type grep
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

Now you can run the following curl commands to simulate different actions:

```
$ curl -i -X POST -d 'json={"action":"login","user":2}' http://localhost:9999/myapp.test
```

```
HTTP/1.1 200 OK
Content-Type: text/plain
Connection: Keep-Alive
Content-Length: 0
```

Since we are currently passing the action as `login`, we can check the logs at the location to verify that they have been printed:

```
2020-12-21 12:31:31.529967000 +0530 myapp.test:
{"action":"login","user":2}
```

If you try to post an action as a `logout` in the following `curl` command, it will get dropped based on the filter rule we specified, and you won’t be able to see any logs being printed:

```
$ curl -i -X POST -d 'json={"action":"logout","user":2}' http://localhost:9999/myapp.test
HTTP/1.1 200 OK
Content-Type: text/plain
Connection: Keep-Alive
Content-Length: 0
```

Now that you understand how Fluentd works, let’s examine how to integrate it in cloud native environments and deploy it over container orchestrators like Kubernetes.

## Fluentd on Kubernetes

By default, in Kubernetes the logs from containers are written to standard output and the standard error stream. This is basic I/O logging, which Kubernetes offers out of the box. The Container engine redirects the streams to a logging driver configured in Kubernetes to write the logs to a file in JSON format. As a user, if you wish to see the logs you can use the `kubectl logs` command. The problem with this approach, though, is very evident: if a container crashes, or a Pod is evicted, or the node itself dies, you will no longer be able to access the application logs. Therefore, logs should have storage that is independent from containers, Pods, and nodes. This concept is formally referred to as *cluster-level logging* and it requires a separate backend to store, analyze, and query the logs. While Kubernetes doesn’t natively provide a solution for cluster-level logging, we can combine Fluentd with a storage solution like Elasticsearch to construct a scalable solution.

Before moving on to the preferred way of deploying Fluentd on Kubernetes, let’s take a look at the different approaches by which cluster-level logging can be implemented, along with their pros and cons. There are essentially two ways to build a cluster-level logging solution:

- Use a sidecar container in every application Pod.
- Use a node-level logging agent that runs on every node.

Let’s take a closer look at each of these solutions.

## Sidecar container approach

Generally, in Kubernetes you run your applications in a Pod, as described in Chapter 5. In the sidecar approach, you can create one or more sidecar containers inside the application Pod. The sidecar container approach can be used in two different ways:

- You can use the sidecar container to stream the application logs to the container’s own stdout.
- You can run a logging agent inside a sidecar container which basically picks the logs from application containers and ships them to a logging backend.

There are some advantages to the sidecar container approach:

- You can ship application logs being produced in different log formats (structured or unstructured) by having two sidecar containers to stream a particular logfile to different log streams.
- Sidecar containers can read logs from files, sockets, or journald, and each sidecar container will print the log to the stdout and stderr
