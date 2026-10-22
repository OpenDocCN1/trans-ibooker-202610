These items most likely have an interesting story behind them where somebody assumed one of these fallacies and found themselves very disappointed. The result wasn’t what they expected and endless hours were lost trying to figure out the wrong problem.

Embracing distributed methodologies is worth the effort in the long run. It is how we build large scale applications and will be for a very long time. The challenge is worth the reward, and for those of us who do this daily, it can be a lot of fun too! Kubernetes applications will test each of these fallacies given its default distributed nature. When you plan your deployment, considering things such as the cost of transport from one place to another or latency implications. They will save you a lot of wasted time and re-design.

# Principles of Cloud Native Data Infrastructure

As engineering professionals, we seek standards and best-practices to build upon. To make data the most “cloud native” it can be, we need to embrace everything Kubernetes offers. A truly cloud native approach means adopting key elements of the Kubernetes design paradigm and building from there. An entire cloud native application that includes data must be one that can run effectively on Kubernetes. Let’s explore a few Kubernetes design principles that point the way.

# Principle 1: Leverage compute, network, and storage as commodity APIs

One of the keys to the success of cloud computing is the commoditization of computation, networking, and storage as resources we can provision via simple APIs. Consider this sampling of AWS services.

*Compute*

We allocate virtual machines through EC2 and Autoscaling Groups (ASGs).

*Network*