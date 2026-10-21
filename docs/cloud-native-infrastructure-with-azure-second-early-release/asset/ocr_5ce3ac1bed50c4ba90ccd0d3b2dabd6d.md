supported by CNCF. Jaeger implements the OpenTracing specification, and its preferred deployment method is Kubernetes. We will keep our discussion limited to Jaeger.

Jaeger closely follows the general architecture of a tracing system in that it consists of three main components: collectors, a query service, and a UI. It deploys an agent on every host, which basically aggregates the trace data and sends it to the collector (buffering service). The trace data then can be stored in a database, preferably Elasticsearch or Cassandra.

Let’s now take a look at some simple ways we can instrument code with different tracing standards and Jaeger.

For instance, we can use a Python Jaeger client:

```
$ pip install jaeger-client
```

From there, we can run all the Jaeger components in one single Docker image:

```
$ docker run -d -p6831:6831/udp -p16686:16686 jaegertracing/all-in-one:latest
```

And we can check for the Docker container using docker ps:

```
$ ~ docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS            PORTS                                                                 NAMES
874b2a03ebaa   jaegertracing/all-in-one:latest   "/go/bin/all-in-one-..."   About a minute ago   Up About a minute   5775/udp, 5778/tcp, 14250/tcp, 6832/udp, 14268/tcp, 0.0.0.0:6831->6831/udp, 0.0.0.0:16686->16686/tcp   jolly_mclean
```

You can hit http://localhost:16686 locally to access the Jaeger UI.

Now let’s discuss how to instrument simple application code with the help of OpenTracing. In the following example, we have a movie-booking program that has a couple of methods which are responsible for checking whether a movie is available. If so, it books the tickets: