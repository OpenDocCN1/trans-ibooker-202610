```
periodSeconds: 15
failureThreshold: 3
```

# Readiness probe

The readiness probe’s responsibility is to identify when a container is ready to serve the user request. A readiness probe helps Kubernetes by not adding an unready Pod’s endpoint to a load balancer too early. A readiness probe can be configured simultaneously with a liveness probe block in the Pod manifest as follows:

```
containers:
  - name: test_image
    image: test_image
    command: ["/bin/sh"]
    args: ['sh', '-c', 'echo Container 1 is Running ; sleep 3600']
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
```

# Startup probe

Sometimes an application requires some additional startup time on its first initialization. In such cases, you can set up a startup probe with an HTTP or TCP check that has a failureThreshold * periodSeconds which is long enough to cover the worst-case startup time:

```
startupProbe:
  httpGet:
    path: /healthapi
    port: liveness-port
  failureThreshold: 30
  periodSeconds: 10
```

# Resource limits

When you deal with Pods, you can specify the resources your application will need. Some of the most basic resource requirements for a Pod to run