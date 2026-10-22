replacing the default kube-scheduler. To demonstrate how, let’s use Figure 9-6 to walk through the components.

![](bbox=[0.12, 0.16, 0.86, 0.49])

Figure 8-6. YuniKorn Architecture

YuniKorn is meant to be a drop-in scheduler replacement with minimal changes to your existing Spark workflow, so we will start there. When new resource requests (1) are sent to the Kubernetes API server via spark-submit (2), the default kube-scheduler (3) is typically used to match Pods and Nodes. When YuniKorn is deployed in your cluster, an admissions-controller Pod is created. The job of the admissions-controller is to listen for new resource requests and make a small change, adding schedulerName: yunikorn to the resource request. If you need more fine-grained control, you can disable the admissions-controller and enable YuniKorn on a per-job basis by manually adding the following line to the SparkApplication YAML.

```
spec:
schedulerName: yunikorn
```

All scheduling needs will now be handled by the YuniKorn Scheduler (5). YuniKorn is built for run with multiple orchestration engines and provides