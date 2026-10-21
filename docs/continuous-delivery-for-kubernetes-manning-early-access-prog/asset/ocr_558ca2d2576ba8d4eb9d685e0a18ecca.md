one is left behind. This promotes the collaboration also on building tooling and more high-level tools for developers and team to use, relying on Kubernetes APIs and not Cloud-Provider specific tools.

+   - Kubernetes offers a transition from On-Prem Cloud and Hybrid to Public Cloud: If you are in a heavily audited environment where you need to keep data inside your country or inside your datacenters, Kubernetes offers you a path to build applications that can be hosted in your Kubernetes On-Prem installation and easily moved to a Public Cloud when Cloud-Providers provides you all the assurances about data and security. Hybrid alternatives, such as running part of your workloads On-Prem and the rest on Public Cloud are also offered by different vendors.

Having listed the good characteristics of Kubernetes, it is also important to recognize that Kubernetes is not a silver bullet and it will not solve all your problems, in some situations, it might cause you more problems on top of the ones that you already have. So, I need to mention when not to use Kubernetes.

# 1.5.2 When not Kubernetes?

Kubernetes is not a silver bullet, and you always need to evaluate if it is worth the effort. There is still a case for heavily adopting Cloud-Provider specific tools to achieve the practices and goals described in this book.

One of the first challenges that you will face when looking to adopt Kubernetes is the steep-learning curve for developers to understand this ecosystem. There are heated discussions around if developers need to actually understand Kubernetes in order to do their daily work. Bringing entire teams up to speed is a challenge, a challenge that can be easily solved by training. Knowledge sharing about a completely new ecosystem and tools is key for organizations to leverage new technologies. It is important to clearly recognize that training needs to happen in your teams to enable your teams to understand and use every day the projects that are covered in this book.

Cases, when you have teams with skills that are related to a particular Cloud-Provider and short deadlines that don’t include Kubernetes training, might push your teams to deliver using Cloud-Provider specific tools.

If you have no reason to move to another Cloud-Provider or if you are already running in your own infrastructure On-Prem, it might be challenging to add Kubernetes into the mix.

When you are evaluating to adopt Kubernetes, it is not the only thing that you need to consider. Kubernetes on its own it is not enough, you need to heavily research which tools are your developers going to use, hence the reason for this book, and how are you going to deliver software.

At the end of the day, Kubernetes is an abstraction, and with abstractions comes lack of specialisation in certain areas. Over the last year, most of these rough edges have been solved by Cloud Providers, giving their Kubernetes users a first-class citizen experience where tools such as centralized logging, monitoring and other cloud-specific tools had been integrated with their Kubernetes managed services.

This book relies on the fact that Kubernetes is adopted by major Cloud-Providers and that you value that for your business. But as mentioned before, Kubernetes is just the starting point, and you need to think through how are your teams going to work with Kubernetes to delivery fantastic software to your customers or users. I hope this book helps you to understand how to evaluate