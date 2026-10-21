
## NOTE

A Dockerfile is technically a mix of both a container runtime and an image specification since it provides semantics about the contents of the image as well as how it is run (i.e., CPU or memory cgroup limitations).

Once you have created your Dockerfile, you can build your container image using the docker build command. This is typically done with the current file path (e.g., docker build), but you may also use a URL (e.g., docker build https://github.com/abcd/docker-example). You can find a full reference on how to use docker build in the documentation.

You should create a tag for your image so that you can reference it quickly and easily in the future when you build the image. You can do this by adding the --tag option when you build:

```
docker build --tag example-flask-container
```

When you run docker build you will see output similar to the following:

```
[internal] load build definition from Dockerfile
=> transferring dockerfile: 203B
[internal] load .dockerignore
=> transferring context: 2B
[internal] load metadata for docker.io/library/ubuntu:18.04
[1/6] FROM docker.io/library/ubuntu:18.04
[internal] load build context
=> transferring context: 953B
CACHED [2/6] WORKDIR /app
[3/6] COPY requirements.txt requirements.txt
[4/6] RUN pip3 install -r requirements.txt
[5/6] COPY . .
[6/6] CMD [ "python3", "app.py"]
exporting to image
=> exporting layers
=> writing image
sha256:8cae92a8fbd6d091ce687b71b31252056944b09760438905b726625831
```
```
564c4c
=> naming to docker.io/library/example-flask-container
```

## Best Practices While Using Docker

The twelve-factor app tracks a number of characteristics of Dockerized and cloud-based applications. This means the application should be able to be deployed, started, stopped, and destroyed without causing problems. While we will talk about container storage solutions later in this book, you should try to avoid persisting data as much as is practically possible unless you’re reading data from a persistent volume or using a cloud native data service (see Chapter 9 for more on this).

There are a few more best practices that you should follow:

- Utilize the docker build options. These options allow you to customize your Docker image to add features such as cgroup controls, tags, network configurations, and more.
- Utilize a .dockerignore file. Similar to .gitignore, a .dockerignore will ensure that certain files or folders are not included in the image build.
- To reduce complexity, dependencies, file sizes, and build times, avoid installing extra or unnecessary packages.
- Use multistage builds. If you need to install dependencies to compile your application, you can do this during the build process of your image, but only copy the output into the container image. This reduces the size of your image build.

You can find more information on Dockerfile best practices on the Docker website.

## Other Container Platforms

While the Docker platform and OCI images remain industry leaders, there are still a number of other container offerings available.

### Kata Containers

Kata Containers are OCI-compliant containers that aim to enhance security. Kata Containers have the following lightweight container features:

- Cgroups
- Namespaces
- Capability filters
- Seccomp filtering
- Mandatory access control

Kata Containers also possess some virtual machine concepts, particularly a separate guest kernel per container and better hardware isolation. The `kata-runtime` process controls the instantiation of the OCI-compliant container. The largest difference is that there is a separate guest kernel per container instead of a shared kernel between container instances.

### LXC and LXD

LXC (Linux Containers) is one of the earliest container mechanisms to be released. LXC mainly uses the kernel’s cgroup and namespace capabilities as well as a standard set of library APIs to control the containers.

LXD is the container management software and tools that help orchestrate the LXC APIs. The LXD daemon provides a REST API over a Unix socket/network that allows you to use the provided command-line tools or build your own.

The ecosystem of both LXC and LXD is more oriented toward running infrastructure than application development due to a lack of development and deployment tooling.

## Container Registries

After building your container images, you will need places to store them and serve them. These are known as *container registries*. Container registries have grown in popularity due to their easy deployment mechanisms and their built-in replication mechanisms which replace the challenging rsync setups required for DEB and RPM package repositories.

When deploying a container, the container deployment system will download the image from the registry to the host machine. Essentially, a container registry acts as a highly available container image file server (with some protections). All of the common containerization platforms allow you to configure a public or private repository as a source for the container images.

While these responsibilities sound reasonably banal, there are a lot of overlooked features of container registries that are important parts of secure container deployments. Such features include:

- *Image replication*
  Replicating the images to other container registries (globally)
- *Authentication*
  Only allowing authorized users to access the registry and its contents
- *Role-based access control*
  Restricting who can modify images and data within the registry
- *Vulnerability scanning*
  Checking images for known vulnerabilities
- *Garbage collection*
  Removing old images regularly
- *Auditing*
  Providing trusted information on changes to the container registry for information security purposes

Current cloud native products include (see the CNCF landscape for more):

- Harbor
- Dragonfly
- Alibaba Container Registry
- Amazon Elastic Container Registry (Amazon ECR)
- Azure Registry
- Docker Registry
- Google Container Registry
- IBM Cloud Container Registry
- JFrog Artifactory
- Kraken
- Portus
- Quay

Many of these systems are tied to specific vendors. However, Harbor is one of the most popular open source container registry systems, so we’ll explore it further.

## Securely Storing Images with Harbor

Harbor is the premier open source cloud native container registry platform. Created in 2016, Harbor stores container images and provides signing and security scanning functionality. Furthermore, Harbor supports high availability and replication functionality as well as a number of security

functions, including user management, role-based access control, and activity auditing.

The beauty of Harbor is that it provides top-tier features even though it is open source, which makes it a great platform to utilize.

## Installing Harbor

When you utilize Harbor, it essentially becomes a critical component of your build and deploy pipelines. You should run it on a host that meets these recommended specifications:

- Quad-core CPU
- 8 GB of memory
- Minimum 160 GB disk
- Ubuntu 18.04/CentOS7 or later

> NOTE
> You will want to evaluate the performance of the disk SKU or configuration to match the performance you desire. Harbor is essentially a fileserver, and the performance of the disk will significantly affect your build and deploy times.

Harbor supports multiple installation methods, including Kubernetes and Helm. As we haven’t yet explored Kubernetes, we are going to install Harbor manually.

Harbor has two releases (or types of installations): an online installer and an offline installer. The online installer is smaller in size and relies on an internet connection to download the full image from Docker Hub (directly off the internet). The offline installer is larger in size and does not require an internet connection. If your security posture is restrictive, the offline installer may be a better option.

The Harbor releases are listed on GitHub.
