# Docker Objects

Docker objects are considered images and containers, which have already been discussed. In short, an image is a template that includes the instructions for how to create a container. While a container is a runnable instance of the image, the container is executed using the Docker client, docker run, which sends a request to the daemon to spin up an instance of the container.

Finally, there are two more topics that require discussion: the Dockerfile and the runtime of the containers. In the next exercise, you will create a Dockerfile and run it in a Linux container. Here is an example of the Dockerfile that you will create later in Exercise 4.2:

```
FROM http://mcr.microsoft.com/dotnet/core/aspnet:2.2
WORKDIR /app
COPY ./publish .
EXPOSE 80
EXPOSE 443
ENTRYPOINT ["dotnet", "csharpguitar-aci.dll"]
```

The FROM instruction identifies the base image, in this case aspnet:2.2. By doing this, you do not need to start from scratch and manually install all the packages required to run this kind of application. If additional or basic packages are required for the application, you would use the RUN instruction and then the package name. WORKDIR is short for working directory. Once this is defined, it is the reference point from which other instructions operate. For example, the point of entry for the COPY instruction is the /app directory. EXPOSE is an important instruction as it informs the users on which port the application will listen. Ports 80 and 443 are common ports for running web applications. ENTRYPOINT is like the Main() method of a console application. This instruction notifies the Docker build where and how to interface with the image once it’s instantiated into a container.

Docker Desktop for Windows supports two container types, Windows and Linux. After installing Docker on your workstation, there will be a white badge that looks like a whale with some ship containers on its back, similar to that shown in Figure 4.13.

# FIGURE 4.13 Docker container operating systems

![](bbox=[0.388, 0.694, 0.6, 0.916])