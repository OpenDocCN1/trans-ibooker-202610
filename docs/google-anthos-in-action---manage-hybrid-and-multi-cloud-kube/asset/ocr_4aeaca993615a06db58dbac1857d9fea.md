| | | | | |
|---|---|---|---|---|
| Unrestricted execution time | Yes* | Yes | Yes | |
| Unrestricted compute/memory limits | Yes** | Yes** | Yes** | |
| Variety of programming languages support | Yes | Yes | Yes | Limited |

*might be restricted for Managed services like Cloud Run.

** depends on the platform

As we can see, the Knative gives you all the advantages of function as a service but also gives you the ability to run your application in almost any language. The limits on the execution time are much higher compared to FaaS. In many cases where there is a need for longer request processing time Knative is a solution. You finally have access to advanced features like volumes and networking so you can tweak your workload if needed. Like with all compute services with higher flexibility the responsibility demarcation line shifts more toward you. You need to build your own container and make sure you are able to make use of all the advanced features but let's be honest, who does not like to be in more control of your application until you get all the benefits of FaaS?

# 11.3.2 Knative History

Google started Knative but now has multiple companies contributing to it like IBM, RedHat, Pivotal, SAP and many others. The full documentation and source code can be found here: https://github.com/knative. Knative started as a set of components that allows the build and run of stateless workloads together with subscription to events. There are currently two active projects in progress on Github those are:

+   - **Knative Serving** - allows to serve serverless containerised workloads