# docker run

This is the command for starting a new container from a base image. You will find that you can retain multiple container images in your local repository that are all based off the same container image. For example, as you add new things into your containers or update the application inside your containers, you may be building new container images that are now a subset of an existing container image. You may have numerous container images that are all named windowsservercore, for example. In this case, container tags become very important, as tags help you to distinguish between different versions of those container images. As an example, here is a command that would start a container based on a windowsservercore image for which I had associated the ltsc2019 tag:

```
docker run -it --rm Microsoft\windowsservercore:ltsc2019
```

In the preceding command, the -it switch creates a shell from which we can interact with a container, which is useful for building and testing containers, but you generally wouldn’t need that switch for launching production containers that were 100% ready to serve up applications. --rm is a cleanup switch, meaning that once this particular container exits, the container and its filesystem will be automatically deleted.

# docker ps -a

You utilize docker ps when you want to view the containers that are currently running on your system.

# Downloading a container image

The first command we will run on our newly created container host is docker images, which shows us all of the container images that currently reside on our system; there are none:

![](bbox=[0.12, 0.62, 0.87, 0.7])

Figure 13.7: No current container images on our system

Of course there are no container images yet, as we haven’t downloaded any. Let’s grab a couple so that we can test this out. There is a sample container image file provided by the .NET team that showcases running a .NET application inside a Nano Server container—that one sounds like a fun way to get started with verifying that I can successfully run containers on this new host server.