
We should also understand that, as our managed instance group grows, the impact of adding the same-sized VM decreases. Let's clarify what we mean here. Say we have set a CPU utilization of 75% and a minimum of three instances in our group. The autoscaler is checking to see whether the aggregate of the VM instances averages above 75%. If the autoscaler adds a fourth VM, it adds a 25% increased capacity to the group.

If we have a maximum of 10 VMs in our group, then the tenth VM is only contributing 10% more capacity – so the impact of the tenth node is not the same as the fourth node. We should keep this in mind when setting utilization and maximum capacity. It's also important to note that autoscaling will always act conservatively and round up the statistics. It would start an extra VM instance that isn't really needed, rather than possibly running out of resources.

Scaling based on HTTP load balancing will scale based on the load of your VM instances. A load balancer will spread the load over backend services that are configured when you create an HTTP load balancer. When instance groups are used for the backend, external load balancers offer two balancing modes – **utilization**, which allows us to specify a maximum target for average backend utilization of all instances in our instance group, and **rate**, where we must specify a target number of requests per second. This is on a per-instance basis or a per-group basis.

The **Target HTTP load balancing utilization** value in the autoscaling policy should be a fraction of what is configured in your load balancer setting. For example, if you set the policy to 100 requests per second, then it would be sensible to scale slightly below this and set the usage in the policy to maybe 80 requests per second (80%):

## New metric

Metric type

HTTP load balancing utilization

Target HTTP load balancing utilization

80 %

Done

Cancel

+ Add new metric

+ Add new scaling schedule

## Predictive autoscaling

Use predictive autoscaling to predict future capacity needs based on historical CPU load.

Learn more

Off
No predictive method is used. The autoscaler scales the group to meet current demand based on real-time metrics.

Optimize for availability
Predictive autoscaling improves availability by monitoring daily and weekly CPU load patterns and scaling out ahead of anticipated demand.

Predictive autoscaling can only be enabled when a CPU utilization metric is applied.

## Cool down period

Specify how long it takes for your app to initialize from boot time until it is ready to serve.

Cool down period

60 seconds

Minimum number of instances

Maximum number of instances

1

10

Figure 4.40 – Autoscaling – HTTP utilization

## Tip

We will discuss load balancers in more detail in Chapter 10, Networking Options in GCP.

We also can scale on Google Cloud operations suite metrics, where the selected metric will provide data for each instance in the managed instance group or the entire group. These metrics can either be out-of-the-box or custom metrics you have created. Note that at the time of writing, in some documentation, the option is still referred to as **Stackdriver** in the settings; however, you should be aware that this was rebranded as Google Cloud's operations suite.

> **Important Note**
> If you have a requirement to scale down to zero instances from time to time, then you should use per-group metrics.

Let's look at another example, using a custom metric that Google offers as a demonstration metric, `custom.googleapis.com/appdemo_queue_depth_01`.

**Utilization target type** is set to **Gauge** in this example, because this will tell the autoscaler to compute the average value of the data collected over the last few minutes and compare it to the target value:

![](img/de2c97ab017f36b3750a5bade111edd7_116_0.png)

Figure 4.41 – Autoscaling – Stackdriver (Google Cloud's operations suite) metrics

Let's assume we now have a startup script on our instance template, which will invoke another script responsible for generating these custom metrics. We can see, from the charts monitoring tab within our instance group, that the load started to increase on our instance groups around 5:55 P.M. but dropped again around 6 P.M.:

![](img/de2c97ab017f36b3750a5bade111edd7_117_0.png)

Figure 4.42 – Monitoring load

Given the spike in our queue depth, we can expect that the **autoscaling policy** would have generated a new instance. If we check the autoscaler logs, we can confirm this. It then added a third instance, before reducing back to two instances when the demand dropped again:

![](img/de2c97ab017f36b3750a5bade111edd7_117_1.png)

Figure 4.43 – Adding and removing instances

This is true elastic computing! These are small examples of how autoscaling works, and we advise that you ensure you can create your own policies and replicate any load that's responsible for increasing and decreasing your machine instances.

**Important Note**
You can only create one autoscaler per managed instance group.

## Autohealing

Autohealing is also part of the managed instance group settings but merits its own section in order to discuss it further. GCP's managed instance groups are responsible for validating whether each VM instance in our group is running and ready to accept client requests. To perform this validation, it needs a health check, which is basically a probe that contacts each member of the instance group to check their current health. The policy can be based on certain protocols, namely HTTP(S), TCP, or SSL.

We also need to configure the criteria to inform the health check how often to check the instance, the acceptable amount of time that it can get a no response, and the number of consecutive failures to its probe. These settings define when a VM would be classified as unhealthy. If that specific instance is no longer healthy, the autoscaler will add a new instance:

![](img/de2c97ab017f36b3750a5bade111edd7_118_0.png)

Figure 4.44 – Autohealing configuration

Within the health checks, we also have to specify the healthy threshold. This figure is used to determine how many continuous successful checks must be returned before an unhealthy instance can be marked as healthy and traffic is directed to it.

We can then apply this health check as an autohealing policy in our instance group. The following screenshot shows the health check selected in this example:

![](img/de2c97ab017f36b3750a5bade111edd7_119_0.png)

Figure 4.45 – Autohealing information

Autohealing policies can also improve the availability of your application. In the next section, we will look at containers on Compute Engine.

## Containers on Compute Engine

As an alternative to running GKE, GCP offers the ability to run containers on Compute Engine. Containers will be covered more in Chapter 6, Managing Kubernetes Clusters with Google Kubernetes Engine; however, it’s important to note that containers can be run on both Linux and Windows server VM instances or container-optimized operating systems offered inside of Compute Engine. Docker and Podman are two common container technologies that let us run containerized applications.

We should note that a Container-Optimized OS does have some limitations:

- It does not include a package manager, so we cannot install software packages directly on an instance.
- It does not support execution of non-containerized applications.
- The kernel is locked down.
- It is not supported outside of the GCP environment.

## Running a simple Windows container on Compute Engine

Let's assume we have a Windows VM up and running with the *Windows Server 2019 DC core for containers* image. We can start RDPing to it, as we did in the *Deploying our first GCE instance* section. As the container is running a core version of Windows, it is running an OS with minimal UI. All we can see is the Command Prompt window.

If we run the `docker images` command, we can see what images are installed by default:

![](img/de2c97ab017f36b3750a5bade111edd7_120_0.png)

Let's create a new image and run it:

- 1. Run the `mkdir my-new-container` command.
- 2. Run the `cd my-new-container` command.
- 3. Run the `mkdir content` command.
- 4. Run the `notepad content\index.html` command.
- 5. Enter the following text to create a simple **Internet Information Services (IIS)** server:

```
<html>
    <head>
        <title>Windows containers</title>
    </head>
    <body>
        <p>This is my test windows container!</p>
    </body>
</html>
```

- 6. Use Notepad to create a file called Dockerfile.
