more about the advanced features of Altair, refer to the official documentation page at https://altair.sirmuel.design/docs/features.

> NOTE
> During a penetration test, you may encounter a remote GraphQL API server with authentication and authorization controls. GraphQL clients such as Altair will need special HTTP headers set so they can authenticate themselves to the remote server. Altair allows you to configure custom headers by using the Set Headers menu item on the left. In the absence of authentication HTTP headers, queries may fail with errors such as 401 Unauthorized. In Chapter 7, we will explore GraphQL servers with authentication and authorization controls enabled.

# Setting Up a Vulnerable GraphQL Server

Now that we have the client utilities needed to query any GraphQL server, the next step is to install a vulnerable GraphQL server, which we’ll use as our target throughout the book. We will use this vulnerable server in our deeper exploration of GraphQL in Chapter 3 and throughout the penetration testing exercises in Chapters 4 through 9.

# Installing Docker

Docker (https://www.docker.com) is a tool for deploying and managing containers. Containers are units of software that package up code and its dependencies so an application can run reliably in various environments. Docker is available on Windows, macOS, and Linux.

We’ll use Docker to deploy the application we’ll attack throughout this book. Let’s install it from the Kali software repositories by running the following command:

```
# sudo apt install -y docker.io
```

Next, we want to make sure the Docker process will automatically start upon system reboot:

```
# sudo systemctl enable docker --now
```

Finally, make sure Docker was successfully installed:

```
# sudo docker

Management Commands:
  builder      Manage builds
  completion   generate the autocompletion script for the specified shell
  config       Manage Docker configs
  container    Manage containers
  context      Manage contexts
```