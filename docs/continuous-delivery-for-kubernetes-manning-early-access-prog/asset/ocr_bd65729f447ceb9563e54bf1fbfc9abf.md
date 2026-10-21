+   - Configuration changes history are managed by Knative Serving: If you change the Knative Service configuration, a new Revision will be created. By default, all traffic will be routed to the latest revision.

Of course, these are the defaults, but you can fine-tune each of your Knative Services to serve your purpose and, for example, implement the previously described release strategies.

In the next section, we will look at how using Knative Serving advanced traffic-handling features can be used to implement Canary Releases, Blue/Green Deployments, A/B testing and Header based routing.

# 5.2.3 Advanced traffic-splitting features

Let’s start by looking at how you can implement a Canary Release for one of our application’s services with a Knative Service.

This section starts by looking into doing Canary Releases using percentage-based traffic splitting. Then it goes into A/B testing by using tag-based and header-based traffic splitting.

# CANARY RELEASES USING PERCENTAGE-BASED TRAFFIC SPLITTING

If you get the Knative Service resource (with `kubectl get ksvc email-service -oyaml`), you will notice that the `spec` section now also contain a `spec.traffic` section that was created by default, as we didn’t specify anything.

```
traffic:
- latestRevision: true
  percent: 100
```

By default, 100% of the traffic is being routed to the latest Knative Revision of the service.

Now imagine that you made a change in your service to improve how emails are sent, but your team is not sure how well it will be received by people and we want to avoid having any backlash from people not wanting to sign in to our conference because of the website. Hence we can run both versions side by side and control how much of the traffic is being routed to each version (Revision in Knative terms).

Let’s edit (`kubectl edit ksvc email-service`) the Knative Service and apply the changes: