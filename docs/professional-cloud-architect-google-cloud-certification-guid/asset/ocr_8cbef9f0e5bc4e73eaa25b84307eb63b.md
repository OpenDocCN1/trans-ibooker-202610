Google supports several service models, including the following:

+   - **Infrastructure-as-a-Service (IaaS)**
+   - **Platform-as-a-Service (PaaS)**
+   - **Container-as-a-Service (CaaS)**
+   - **Function-as-a-Service (FaaS)**
+   - **Managed services**

As you can see, the range of services in GCP is very broad. Let's quickly analyze this range of services offered by GCP. We will start from very simple IaaS, such as a traditional data center, and end with using FaaS, where we can run code without the need to manage any server infrastructure. The choice of service depends on our requirements. Put simply, if we require flexibility and control over our **Virtual Machines (VMs)**, we would use **Compute Engine**. This service allows us to provision VM instances or simply lift and shift machines from our existing environment. However, the trade-off is that you are responsible for managing all of the layers above the VM instance. That includes the operating system, any middleware, and any applications on top of it.

When we move away from IaaS toward PaaS, CaaS, or FaaS, the responsibility of maintaining the infrastructure is taken away from us. With **Cloud Functions**, all we really care about is the coding of a function in a language supported by GCP. Once it's done and published, we access it through the HTTP(S) protocol.

Finally, as we move to managed services, we simply start to consume services that bring us a particular business value without having to worry about any underlying parts. They can be used in **Software-as-a-Service (SaaS)** models and consumed through APIs. An example of this is **Dataprep**, which is a data service that allows you to clean up and prepare your data for further analysis. Another example is the pretrained ML model, **Vision API**. Developers can consume this service by using the RESTful API to analyze images without having to write any code, except for the call itself.

Hopefully, now you understand that GCP is much more than just a hosting service. It provides you with sets of tools, services, and resources that will help you to develop and deliver your applications. The choice of the services you will use depends entirely on the set of requirements you have. If that feels overwhelming, then don't worry. This book is written to help you to go through GCP step by step.

In *Chapter 3, Google Cloud Platform Core Services*, you will get an overview of the most important GCP services. In the following chapters, we will dive into each of them in more detail to get you prepared for the exam.