
![](img/de2c97ab017f36b3750a5bade111edd7_545_0.png)

Figure 16.28 – Pricing Calculator – Storage

We encourage you to navigate to this website and familiarize yourself with different examples.

## Additional things to consider

There are additional management tools that we need to consider. For the exam, it is also important to be aware of the following:

- **Cloud Source Repositories**: These are fully featured GitHub repositories that are hosted on GCP and support the collaborative development of an application. You can use the `gcloud` commands to create code, commit code, and manage new repositories. More information can be found at https://cloud.google.com/source-repositories/docs/.
- **Cloud Build**: This is a GCP CI/CD service that will execute your builds on the GCP infrastructure. It can import source code from Google Cloud Storage, Cloud Source Repositories, GitHub, or Bitbucket. More information can be found in Chapter 6, Managing Kubernetes Clusters with Google Kubernetes Engine, and here: https://cloud.google.com/cloud-build/docs/.
- **Cloud Scheduler**: This is a fully managed cron job scheduler service. It can be used to trigger jobs on App Engine, send Pub/Sub messages, or even hit HTTP(S) endpoints.
- **Container Registry**: This provides us with private Docker repository storage on our GCP platform. We can use the `gcloud` commands to push images to our registry, and then we can pull these images using an HTTP endpoint from any GCE instance or even our hardware. More information can be found in *Chapter 6, Managing Kubernetes Clusters with Google Kubernetes Engine.*
- **Cloud Endpoints**: This acts as an API management system that allows us to manage our APIs on any Google Cloud backend. Once you have deployed your API in Cloud Endpoints, you can create a developer portal that users can interact with and access and view documentation.

## Summary

In this chapter, we looked over the main management tools that can be used to manage GCP services. It's important to reiterate that you should understand which services can be managed by which command-line tool and the basic command-line structure. Let's review what we would use each tool for:

- `gcloud` can be used to manage many GCP services, but it is most commonly used to manage Compute Engine VM instances, Cloud SQL instances, Kubernetes Engine clusters, and Cloud Deployment Manager deployments.
- Use `cbt` to manage Bigtable.
- Use `bq` to manage BigQuery.
- Use `gsutil` to manage Cloud Storage. We need to understand the different syntax for creating buckets, copying data, and removing buckets.
- Deployment Manager can be used to automate a variety of GCP services within templates and configuration files.
- Pricing Calculator.

Remember that Google offers a free tier where you can use these command-line tools and familiarize yourself with creating and managing different services.

In the next chapter, we will look at how to monitor our GCP services.

## Further reading

Read the following articles for more information about what was covered in this chapter:

- **APIs Explorer:** https://developers.google.com/apis-explorer/#p/
- **gsutil:** https://cloud.google.com/storage/docs/gsutil
- **cbt:** https://cloud.google.com/bigtable/docs/quickstart-cbt
- **gcloud:** https://cloud.google.com/sdk/gcloud/
- **bq:** https://cloud.google.com/bigquery/docs/bq-command-line-tool
- **Cloud Deployment Manager:** https://cloud.google.com/deployment-manager/docs/

## 17
Monitoring Your Infrastructure

In this chapter, we will look at GCP monitoring. We will discuss what Google Cloud's operations suite (formerly known as Stackdriver) is and why you need it. We will explain all of the services that it offers. We will also go through some basic configurations that will give you a good understanding of how you can leverage it in order to monitor your Google Cloud services, such as Google Compute Engine (GCE) or Google Kubernetes Engine (GKE).

The following topics will be covered in this chapter:

- Introduction to Google Cloud's operations suite
- Configuring Google Cloud's operations suite
- Cloud Monitoring
- Cloud Logging
- Application Performance Management (APM)

**Exam Tips**
Monitoring is a massive topic and can feel somewhat overwhelming. Once you understand the three main functionalities of Google Cloud's operations suite, which are monitoring, logging, and APM, we want you to focus on what you will be tested on in the exam. Make sure that you know which GCP services you can monitor with Google Cloud's operations suite. Pay special attention to GCE, both in terms of performance and availability monitoring. Understand audit logs well, and make sure that you know how to track who made the changes to your project and the associated resources. Log in to the GCP console and browse the activity and audit logs. Anticipate that you may be asked about where to find particular types of logs! Finally, Qwiklabs will allow you to understand the service better, and we strongly recommend that you have a look at the available labs.

## Technical requirements

In order to gain hands-on experience with Google Cloud's operations suite, we recommend that you use Qwiklabs. The Google Cloud operations suite quest can be found here: https://www.qwiklabs.com/quests/35?locale=en.

**Important Note**
You need to purchase credits in order to enroll for the quest.

## Introduction to Google Cloud's operations suite

We have already learned about a number of services, and now it is time to take a closer look at the monitoring options. Google comes with a GCP-integrated tool called Google Cloud's operations suite (formerly known as Stackdriver). Stackdriver was founded in 2012 as a Software-as-a-Service (SaaS) platform that would allow consistent monitoring across different cloud layers. In 2014, it was acquired by Google and made publicly available in 2016 as a GCP service.

Currently, Google Cloud's operations suite can monitor both GCP and AWS. However, integration with the former is much more robust. Google Cloud's operations suite is a set of tools that provides the following services:

- Monitoring
- Logging
- Debugging
- Tracing
- Error Reporting
- Profiling

Monitoring and Logging are agentless, but in order to obtain more informative metrics and logs, an agent should be installed. There are actually two types of agents that can be installed on your instances: a Monitoring agent, based on the `collectd` software, and a Logging agent, based on the `fluentd` software.

In the following diagram, you can see a schema, which highlights a GCE VM instance and the agents that are installed on top of it. The VM instance, by default, talks to Logging and Monitoring. The agents report to their respective services:

![](img/de2c97ab017f36b3750a5bade111edd7_551_0.png)

Figure 17.1 – Google Cloud's operations suite

GCP is a developer-oriented platform, and it provides a lot of monitoring-related services that can be directly integrated with your application. Google Cloud's operations suite allows you to monitor both the infrastructure and the applications. It is very important to understand that developers need to consider monitoring and logging from the very outset when designing applications. For each of the programming languages that are supported, there are libraries that facilitate log integration with Google Cloud's operations suite. Make sure that you check out the languages that are supported so that you can take full advantage of them!

In this chapter, we will have a look at how Google Cloud's operations suite is used to monitor GCP. Remember to read this chapter carefully and perform the recommended Qwiklabs, as this is a key exam topic.

Now that we have a basic understanding of what Google Cloud's operations suite is, let's have a look at the cost of each service.
