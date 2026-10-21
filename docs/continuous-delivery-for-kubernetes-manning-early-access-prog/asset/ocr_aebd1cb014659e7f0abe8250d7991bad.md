# BLUE/GREEN DEPLOYMENTS

For situations where we need to change from one version to the next at a very specific point in time, because there is no backward compatibility we can still use tag based routing with percentages. Instead of going gradually from one version to the next, we use percentages as a switch going from 0 to 100 on the new version and from 100 to 0 on the old version.

Most Blue/Green deployment scenarios require coordination between different teams and services to make sure that both the service and the clients are updated at the same time. Knative Serving allows you declaratively define when to do the switching from one version to the next in a controller way.

![](bbox=[0.18, 0.35, 0.88, 0.68])

Figure 5.14 Blue/Green Deployments using Knative Serving tag-based routing

To achieve this scenario described in figure 5.14, we can create the "green" tag for the new version inside our Knative Service as follows:

```
...
traffic:
  - revisionName: <first-revision-name>
    percent: 100 # All traffic is still being routed to the first revision
  - revisionName: <second-revision-name>
    percent: 0 # 0% of traffic routed to the second revision
    tag: green # A named route
```