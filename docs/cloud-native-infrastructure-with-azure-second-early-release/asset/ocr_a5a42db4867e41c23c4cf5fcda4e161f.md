```
Deployed.
200 OK
URL: http://127.0.0.1:31112/function/hello-python
```

+   - The previous step outputs a URL you can test against. Use `curl` to test it:

```
$ curl -X POST 127.0.0.1:31112/function/hello-world -d
"My name is Michael"
Hello world! My name is Michael
```

By default, OpenFaaS uses requests per second (rps) as a mechanism to perform autoscaling. You can find more information about OpenFaaS autoscaling configuration on the Autoscaling documentation page.

At this time, you will have a running OpenFaaS function on Kubernetes. You can also run OpenFaaS outside of Kubernetes using faasd. There’s more information on the OpenFaaS deployment page.

Finally, OpenFaaS is built on top of the Kubernetes HPA. You can find out more about how to load-test and autoscale your OpenFaaS function from the OpenFaaS Kubernetes tutorial page.

# Summary

While serverless is definitely not for everyone, when used in the right situations it can provide significant upsides. The serverless space is still evolving, but the barrier to entry is extremely low, which makes it an exciting technology. In this chapter, we explored four different ways you can run serverless functions on Azure. First we looked at Azure function apps and then we looked at three leading CNCF projects: Knative, KEDA, and OpenFaaS. At this point, we have outlined the major pillars of cloud native infrastructure. In Chapter 12, we will recap what you learned throughout this journey.