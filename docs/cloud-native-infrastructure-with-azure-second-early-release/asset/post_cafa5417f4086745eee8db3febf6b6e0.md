
```
$ kubectl apply -f keda-config/jobs/secret.yaml
$ kubectl apply -f keda-config/jobs/keda.yaml
```

+   - Verify that your scaled job is ready by running:

```
$ kubectl get ScaledJob
$ kubectl get TriggerAuthentication
```

+   - You can test your KEDA deployment by running a test script. First you need to set up the Python environment:

```
$ python3 -m venv .
$ source bin/activate
$ pip3 install logger
$ pip3 install azure-servicebus
```

+   - Now you need to create your program that sends messages to the Azure Service Bus queue. You will need to modify connection_string and queue_name:

```
import os
import sys
import time
import yaml
from logger import logger
from azure.servicebus import ServiceBusClient,
ServiceBusMessage
```

```
def send_a_list_of_messages(sender):
    messages = [ServiceBusMessage(f"Message in list {i}")
    for i in range(1000)]
    sender.send_messages(messages)
    logger.info("Sent a list of 1000 messages")
```

```
def main():
    with open("azure-service-bus.yaml", 'r') as stream:
        config = yaml.safe_load(stream)

    logger.info("Start sending messages")
    connection_string = <FIXME>
    queue_name = <FIXME>

    queue =
ServiceBusClient.from_connection_string(conn_str=connecti
on_string, queue_name=queue_name)

    with queue:
        sender =
queue.get_queue_sender(queue_name=queue_name)
        with sender:
            send_a_list_of_messages(sender)

    logger.info("Done sending messages")
    logger.info("------------------------")

if __name__ == "__main__":
    main()
```

+   - Run *python3 send_message_queue.py*. This will create 1,000 messages and send them to the Azure Service Bus queue.

You can see the messages being ingested and then processed in the Azure Service Bus queue (Figure 11-4).

![](img/8d47d1b5dbca55e784ae602145a894c7_378_0.png)

Figure 11-4. Azure Service Bus queue

In the next section, we will look at OpenFaaS, which provides simple, no-frills functions on both standalone systems and Kubernetes clusters.

## OpenFaaS

OpenFaaS is another event-driven function service for Kubernetes that allows you to write functions in any language and then package them in Docker or OCI-compatible containers.

### OpenFaaS Architecture

The OpenFaaS reference architecture (as shown in Figure 11-5) is reasonably similar to KEDA. However, OpenFaaS does rely on metrics from Prometheus to act as an autoscaling signal.

![](img/8d47d1b5dbca55e784ae602145a894c7_378_1.png)

Figure 11-5. The OpenFaaS reference architecture

## Installing OpenFaaS

OpenFaaS has three recommended installation methods:

- Using `arkade`
- Using `helm`
- Installing with Flux or ArgoCD

To be consistent with the rest of the book, we’ll deploy OpenFaaS using Helm.

- Create the namespaces (one for OpenFaaS core services and one for functions):

```
kubectl apply -f
https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

- Add the OpenFaaS Helm chart:

```
helm repo add openfaas https://openfaas.github.io/faas-netes/
```

- Finally, deploy OpenFaaS:

```
helm repo update \
  && helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas \
    --set functionNamespace=openfaas-fn \
    --set generateBasicAuth=true
```

- You’ll be able to retrieve your administrator password by running the following command; this will be used for accessing the OpenFaaS dashboard:

```
PASSWORD=$(kubectl -n openfaas get secret basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode)
&& \
echo "OpenFaaS admin password: $PASSWORD"
```

Now that we have OpenFaaS installed, we’re going to write an OpenFaaS function.

## Writing Your First OpenFaaS Function

In this section, we will utilize the faas-cli to quickly create and deploy a serverless function.

- Download the faas-cli by running `brew install faas-cli` on OSX, or by running `curl -sSL https://cli.openfaas.com | sudo sh` on Linux.
- Create a folder to place your new function:
- Use the faas-cli to create a new Python function project:

```
mkdir -p ~/functions && cd ~/functions
```

```
faas-cli new --lang python helloworld
```

This will create three new files:

```
$ ls
helloworld/handler.py
helloworld/requirements.txt
helloworld.yml
```

The handler.py file will be where you write your function. The requirements.txt file is the same as a standard Python

*requirements.txt file*. The *helloworld.yml* file defines some boilerplate for your function.

- In the *handler.py* file, we will define our function. This function will print "Hello World! You said: " plus the data posted to the function endpoint:
  
  ```
  def handle(req):
      print("Hello World! You said: " + req)
  ```

- Finally, in your *helloworld.yml* file, you will define metadata about your function. http://127.0.0.1:31112 is the default address of the OpenFaaS gateway/API server:
  
  ```
  provider:
    name: openfaas
    gateway: http://127.0.0.1:31112
  
  functions:
    hello-python:
      lang: python
      handler: ./hello-world
      image: hello-world
  ```

- Build the serverless image and push it to the Harbor registry you created in Chapter 2:
  
  ```
  $ faas-cli build -f ./hello-world.yml
  $ faas-cli push -f ./hello-world.yml
  ```

- Deploy it to the Kubernetes cluster:
  
  ```
  $ faas-cli deploy -f ./hello-world.yml
  Deploying: hello-python.
  No existing service to remove
  ```

```
Deployed.
200 OK
URL: http://127.0.0.1:31112/function/hello-python
```

- The previous step outputs a URL you can test against. Use `curl` to test it:

```
$ curl -X POST 127.0.0.1:31112/function/hello-world -d
"My name is Michael"
Hello world! My name is Michael
```

By default, OpenFaaS uses requests per second (rps) as a mechanism to perform autoscaling. You can find more information about OpenFaaS autoscaling configuration on the Autoscaling documentation page.

At this time, you will have a running OpenFaaS function on Kubernetes. You can also run OpenFaaS outside of Kubernetes using faasd. There’s more information on the OpenFaaS deployment page.

Finally, OpenFaaS is built on top of the Kubernetes HPA. You can find out more about how to load-test and autoscale your OpenFaaS function from the OpenFaaS Kubernetes tutorial page.

## Summary

While serverless is definitely not for everyone, when used in the right situations it can provide significant upsides. The serverless space is still evolving, but the barrier to entry is extremely low, which makes it an exciting technology. In this chapter, we explored four different ways you can run serverless functions on Azure. First we looked at Azure function apps and then we looked at three leading CNCF projects: Knative, KEDA, and OpenFaaS. At this point, we have outlined the major pillars of cloud native infrastructure. In Chapter 12, we will recap what you learned throughout this journey.

## Chapter 12. Conclusion

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 12th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

We’ve come to the end of the book, so let’s recap our journey. Throughout this book, we aimed to provide a single resource that you can use to build and run a cloud native infrastructure in Azure. Although Azure has been our focus, you can also apply the same knowledge to build and operate your own cloud native environment over different cloud vendor platforms. Let’s go through what you’ve learned so far.

We began this book by looking back at how the world of infrastructure and services evolved over time with the introduction of the cloud. To take full advantage of what the cloud has to offer, we need to develop solutions that are native to the cloud and not just retroactively adapt them for the cloud. We understood how cloud native is the way to go in order to fully utilize the power of the cloud.

In Chapter 2, we introduced the steppingstones to building a modern cloud native environment using Azure. We set up an Azure account and got started with Ansible, Terraform, and Packer. By employing them as our key tools, we can enable production-ready services in an automated fashion. We
