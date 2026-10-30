
11. We will use the default trigger in this case, so we can click on the DEPLOY button. After a couple of seconds, we will see that a single revision of the service was deployed. All the requests will be directed to that revision. Let's click on the URL link and see whether the application is available on the internet:

![](img/de2c97ab017f36b3750a5bade111edd7_225_0.png)

Figure 7.14 – Deployed Cloud Run service

As we can see, the application has been successfully deployed and can be accessed from the internet:

![](img/de2c97ab017f36b3750a5bade111edd7_226_0.png)

Figure 7.15 - Deployed Application

Congratulations – you have deployed your first Cloud Run service! Now, let's learn how to deploy a new revision of the application.

## Deploying a new revision

To deploy a new revision of the application, return to the Cloud Run service details and follow these steps:

- 1. Click on the **EDIT & DEPLOY NEW REVISION** button:

![](img/de2c97ab017f36b3750a5bade111edd7_227_0.png)

Figure 7.16 – EDIT & DEPLOY NEW REVISION

- 2. Let's choose one of the **Capacity** parameters to change:

### Capacity

Memory allocated
512 MiB
Memory to allocate to each container instance.

CPU allocated
1
Number of vCPUs allocated to each container instance.

Request timeout
300 seconds
Time within which a response must be returned (maximum 3600 seconds).

Maximum requests per container
80
The maximum number of concurrent requests that can reach each container instance. What is concurrency?

Figure 7.17 – Changing the capacity

3. We will edit the **Maximum requests per container** attribute and change it from 80 to 1. This means that each container instance will only be able to serve a single request. By default, each container can serve 80 requests. We will leave the **Serve this revision immediately** option as is so that all the traffic will be directed to the new revision instead of using existing splits. Click on the **DEPLOY** button:

> **Exam Tip**
> Concurrency is one of the differentiators regarding Cloud Run and Cloud Functions. In Cloud Function, each function execution can only serve a single request.

### Capacity

Memory allocated
512 MiB
Memory to allocate to each container instance.
CPU allocated
1
Number of vCPUs allocated to each container instance.

Request timeout
300 seconds
Time within which a response must be returned (maximum 3600 seconds).

Maximum requests per container
1
The maximum number of concurrent requests that can reach each container instance. What is concurrency?

### Autoscaling

Minimum number of instances *
0
Maximum number of instances
100

Serve this revision immediately
100% of the traffic will be migrated to this revision, overriding all existing traffic splits, if any.

DEPLOY   CANCEL

Figure 7.18 – Maximum requests per container equals 1

4. After waiting a couple of seconds, you will see that a new revision has been deployed. Note that 100% of the traffic is automatically routed to the new revision:

![](img/de2c97ab017f36b3750a5bade111edd7_229_0.png)

Figure 7.19 – New revision deployed

5. If we click on the MANAGE TRAFFIC link, a menu will appear. In this menu, you can set a custom amount of percentage for traffic to a specific revision. This might be useful for performing tests and rollbacks. Set this as a 50:50 split and click the SAVE button:

![](img/de2c97ab017f36b3750a5bade111edd7_229_1.png)

Figure 7.20 – Manage traffic

6. In the **Service details** section, we will see that the traffic has been split between two revisions:

![](img/de2c97ab017f36b3750a5bade111edd7_230_0.png)

Figure 7.21 – Service details

With that, we have learned how to deploy new revisions of a Cloud Run service and how to manage the traffic to route a certain amount of traffic to a particular revision. This can, for example, help you run canary deployments on a small set of users. If you want to learn how to deploy a new revision using continuous deployment, visit Konrad's YouTube channel: https://www.youtube.com/watch?v=QjBbOKb9yns.

## Mapping custom domains

As we saw in the previous sections, for each of the Cloud Run services that are deployed, an automatic URL is generated. You might want to map your domain instead of using the generated one. This is possible with the mapping custom domains feature. It requires that you do the following:

- Map a custom domain to a service.
- Add the DNS records to your domain registrar.

If you do not own a domain, you can register a domain with the Cloud Domains service directly from the Cloud Run console.

Note that this feature is only available in a few regions. To find the list of all the supported regions and see the step-by-step procedure for mapping the custom domains, check out https://cloud.google.com/run/docs/mapping-custom-domains.

## IAM roles

Access to Cloud Run is secured with IAM. Let's have a look at a list of predefined roles, along with a short description of each:

- **Cloud Run admin** (`roles/run.admin`): Has the right to create, update, and delete services. You can also get and set IAM policies. Needs additional permission to deploy services.
- **Cloud Run developer** (`roles/run.developer`): Has the right to create, update, and delete services. Can also get but *not* set IAM policies.
- **Cloud Run viewer** (`roles/run.viewer`): Has the right to view services and get IAM policies.
- **Cloud Run invoker** (`roles/run.invoker`): Has the right to invoke services.

## Quotas and limits

The list of quotas and limits for Cloud Run is quite long, so please consult https://cloud.google.com/run/quotas for a detailed list. For the exam, you might want to remember some of the most important ones.

Cloud Run comes with the following limits:

- 1,000 services per region
- 1,000 revisions per service
- 1,000 container instances per service (extendable)
- 8 GB memory per container instance
- 4 vCPUs per container instance
- 8 GB writable in-memory storage
- 60-minute maximum timeout
- 250 concurrent requests per container instance

## Exam Tips

Remember that Cloud Run has a 60-minute timeout, while Cloud Function has a 9-minute (540s) timeout. This means that Cloud Run can be a solution for cases where Cloud Function would time out. However, keep the cost aspects of long-running container instances in mind. Also, Cloud Run can serve up to 250 concurrent requests while Cloud Function can only serve a single thread.

## Pricing

As Cloud Run is a fully managed service, you are charged per use and do not have to worry about the underlying infrastructure. It is quite important to understand what this means as we have learned that containers are provisioned and de-provisioned as requests to access the applications are triggered. In the case of Cloud Run, the charge applies when the application is running, and you are billed to the nearest 100 milliseconds. A charge applies to the time when the following occurs:

- The container instance is starting.
- The container instance is shut down and it is handling the SIGTERM (https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-terminating-with-grace) signal.
- At least one instance is serving the request.

The active billable time begins with the start of the first request and ends at the end of the last request:

![](img/de2c97ab017f36b3750a5bade111edd7_232_0.png)

Figure 7.22 – Cloud Run billing time

Cloud Run comes with a free tier of 2 million requests and the first million have 180,000 vCPU per second and 360,000 GiB per second of memory. There is also a free egress of 1 GiB within North America.

Note that if you set a minimum number of container instances, you will be always billed for those running containers. By default, your Cloud Run service scales to zero and there is no charge for idle time.

## Summary

In this chapter, we learned about Cloud Run, which is a service that allows you to run cloud-native workloads on Google Cloud serverlessly. Cloud Run abstracts all the implementation details of Kubernetes and Knative away from the developer and allows a seamless deployment experience. The developer does not need to understand the complex Kubernetes resource definitions to define how the application should behave under load. Scaling and load balancing are done under the hood based on the parameters that are defined during the deployment. Cloud Run integrates with many GCP services such as Cloud IAM and the Cloud operations suite for logging and monitoring.

We also deployed the Cloud Run service and learned how new revisions can be deployed. Finally, we learned how to control traffic between various revisions.

In the next chapter, we will talk about Anthos, a GCP hybrid multi-cloud platform that allows you to manage cloud-native workloads both in the public cloud as well as in on-premises data centers.

## Further reading

For more information on the topics that were covered in this chapter, take a look at the following resources:

- **Overview:** https://cloud.google.com/run/docs
- **Developing the Cloud Run service:** https://cloud.google.com/run/docs/developing
- **Deploying to Cloud Run:** https://cloud.google.com/run/docs/deploying
- **Monitoring Cloud Run:** https://cloud.google.com/run/docs/monitoring
- **Cloud Run authentication:** https://cloud.google.com/run/docs/authenticating/overview
- **Limits and quotas:** https://cloud.google.com/run/quotas
- **Pricing:** https://cloud.google.com/run/pricing
