
Containerd is an OCI-compliant container runtime that is used in Docker. Containerd acts as a layer of abstraction between all the Linux syscalls that make a container operate and the standard OCI configuration that configures a container. Containerd has an events subsystem that allows other systems like etcd to subscribe to system changes and act accordingly. Notably, containerd supports both OCI and Docker image formats. What is containerd? is an excellent blog post with further information.

## CRI-O

CRI-O is an implementation of the Kubernetes Container Runtime Interface (CRI) that enables OCI-compatible runtimes. It is considered to be a lightweight alternative to using Docker and containerd. CRI-O also supports running Kata Containers (a virtual-machine-like container created by VMware) and is extensible to any other OCI-compliant runtime. This makes it an attractive runtime to use for a number of use cases.

## Docker

Not to be confused with the complete Docker platform, Docker actually contains a runtime engine which ironically is essentially containerd. Kubernetes supported Docker as a container runtime until the v1.20 release of Kubernetes.

## Containers

The container instance is the deployment and operation of software as defined by a container specification. The container runs the defined software (usually provided by an image) contained by a number of settings and limits.

## Operating System

The operating system is the lowest of the constructs, which is what our containers run on. In most container runtimes, the kernel is shared among all container instances.

## Open Container Initiative (OCI) Specification

As mentioned earlier, containers were not a well-defined concept in the beginning. The Open Container Initiative (OCI) was established in 2015 to formalize the aforementioned runtime and image specifications.

A container’s runtime configuration is specified in a config.json file that is used by the container runtime to configure the container’s operational parameters. The use of an OCI-compliant container allows the same container image/configuration to be run on multiple container orchestrators (e.g., Docker and rkt) without the need to modify anything.

## OCI Image Specification

The OCI image specification outlines how the layers of the container are defined. Some of the properties of the image include:

- Author
- Architecture and operating system
- User/group to run the container as
- Ports that are exposed outside the container
- Environment variables
- Entry points and commands
- Working directory
- Image labels

The following is an example of a container image:

```
{
  "created": "2021-01-31T22:22:56.015925234Z",
  "author": "Michael Kehoe <michaelk@example.com>",
  "architecture": "x86_64",
  "os": "linux",
  "config": {
```

```
"User": "alice",
"ExposedPorts": {
    "5000/tcp": {}
},
"Env": [
"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    ],
"Entrypoint": [
    "/usr/bin/python"
],
"Cmd": [
    "app.py"
],
"Volumes": {
    "/var/job-result-data": {},
    "/var/log/my-app-logs": {}
},
"WorkingDir": "/app",
"Labels": {
    "com.example.project.git.url":
"https://example.com/project.git",
    "com.example.project.git.commit":
"45a939b2999782a3f005621a8d0f29aa387e1d6b"
    }
},
"rootfs": {
    "diff_ids": [
"sha256:c6f988f4874bb0add23a778f753c65efe992244e148a1d2ec2a8b664fb66bbd1",
"sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef"
    ],
    "type": "layers"
},
"history": [
    {
        "created": "2021-01-31T22:22:54.690851953Z",
        "created_by": "/bin/sh -c #(nop) ADD file:a3bc1e842b69636f9df5256c49c5374fb4eef1e281fe3f282c65fb853ee171c5 in /"
    },
    {
        "created": "2021-01-31T22:22:55.613815829Z",
        "created_by": "/bin/sh -c #(nop) CMD [\"sh\\\"]",
```

```
"empty_layer": true
}
]
}
```

## OCI Runtime Specification

At a high level, the runtime config specification provides control of the following container attributes:

- Default filesystems
- Namespaces
- User ID and group ID mappings
- Devices
- Control group configuration
- Sysctl
- Seccomp
- Rootfs mount
- Masked and read-only paths
- Mount SELinux labels
- Hook commands
- Other specialized attributes (e.g., personality, unified, and Intel RDT)

The OCI specification also covers how the state of a container is defined and the lifecycle of the container.

The state of the container can be queried by a state function (in containerd, in and runc) and returns the following attributes:

```
{
    "ociVersion": "0.2.0",
```

```
"id": "oci-container1",
"status": "running",
"pid": 4422,
"bundle": "/containers/redis",
"annotations": {
    "myKey": "myValue"
}
}
```

The lifecycle of the OCI container brings together all the runtime specifications, including how the runtime specification interacts with the image specification. It defines how the container is started and configured, and then how it interacts with the image start/stop hooks.

## Docker

Docker provides a platform as a service (PaaS) that enables users to run containerized applications on a desktop in a simplified and standardized manner. Docker runs images that are OCI compliant and is often a means to test a code deployment before deploying in a production environment. Furthermore, Docker’s general availability of desktop software provides developers an easy mechanism to test the packaging and deployment of their code, which has made it an attractive system for developers.

## Building Your First Docker Image

Docker images by themselves are non–OCI-compliant images (their structure is different from the OCI specification) that run on Docker and on systems that support the Docker runtime (containerd). The beauty of Docker images is in how quick and simple they are to create. Each container image is defined by a Dockerfile. This file consists of properties and commands for building the image, running the application, exposing it to the network, and performing health checks. It defines what dependencies and files are copied into the container image as well as what commands are executed to start the applications within the container. This approach of building the Docker image with a simple (imperative) shell-script-like

format (running command after command) was preferred by developers who were dealing with painful package management or bundling applications in RPM or DEB (or tarballs).

Docker provides a full configuration reference which may assist you in building your own container.

## BUILDING YOUR OWN CONTAINER

A great way to understand how containers work is to build your own basic container. We recommend trying the rubber-docker workshop. It walks through the basic components of a container and shows how they are implemented.

While the Dockerfile allows for complex setups, it is exceptionally easy to create an image in just a few lines. In the following example, we’ll run a Python Flask web application and expose port 5000.

- Create a requirements.txt file:

```
Flask >2.0.0
```

- Create an app.py file:

```
from Flask import flask

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello, World"
```

- Create your Dockerfile, called Dockerfile:

```
1. FROM ubuntu:18.04
2. COPY . /app
```

```
3. WORKDIR /app
4. RUN pip3 install -r requirements.txt
5. ENTRYPOINT ["python3"]
6. CMD ["app.py"]
7. EXPOSE 5000
```

- On line 1, we take a base image to use. In this case, we’re going to use Ubuntu 18.04 from the Docker Hub registry.
- On line 2, we copy the current directory into a new image directory called *app*.
- On line 3, we set the current working directory to */app*.
- On line 4, we use `pip` to install the application’s defined dependencies.
- On line 5, we set the entrypoint application as `python3` (this is a Docker/Python best practice).
- On line 6, we start the application by running *app.py*.
- On line 7, we expose port 5000 services outside the container.

In its entirety, this container image copies over our application and then installs the application’s requirements, starts itself, and allows network connections on port 5000 (the default Flask port).

To allow users or other applications to access your container, the Dockerfile contains a directive, `EXPOSE`, that allows the container network stack to listen on the specified port at runtime. We’ll discuss network connectivity in Chapter 8.
