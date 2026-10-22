As you can see, installing Pulsar has created a lot of infrastructure, with a lot of communication between the elements. High traffic volume is a typical situation. When we build out virtual data centers in Kubernetes, it will create a lot of internode and external network traffic. All traffic should be encrypted with Transport Layer Security (TLS) and Secure Socket Layer (SSL) using x.509 certificates. The most important part of this system is the Certificate Authority (CA). In a Public Key Infrastructure (PKI) arrangement acts as a trusted third party that digitally signs certificates used to create a chain of trust between two entities. Going through the procedure to have a certificate issued by CA historically has been a manual and arduous process, which unfortunately has led to a lack of secure communications in cloud-based applications.

Cert-manager is a tool that uses the Automated Certificate Management Environment (ACME) protocol to add certificate management seamlessly to your Kubernetes infrastructure. We should always use TLS to secure the data moving from one service to another for our streaming application. The cert-manager project is arguably one of the most critical pieces of your Kubernetes infrastructure that you will eventually forget about. That’s the hallmark of a project that fits the moniker of “it just works.”

# WHAT IS ACME?

When working with x.509 certificates, you’ll frequently see references to the Automated Certificate Management Environment (ACME). ACME allows for automated deployment of certificates between user infrastructure and certificate authorities. It was designed by the Internet Security Research Group when they were building their free certificate authority, Let’s Encrypt. It would be putting it lightly to say this fantastic free service has been a game-changer for cloud native infrastructure.

Adding TLS to your Pulsar deployment has been made incredibly easy with just a few configuration steps. Before installing Pulsar, you’ll need to set up the Cert-manager service inside the target Kubernetes cluster. First, add the Cert-manager repo to your local Helm installation.

```
helm repo add jetstack https://charts.jetstack.io
```