# docker version

Certain container images that you want to run may require particular versions of Docker Engine to be successful. When in doubt of what your current Docker environment is running, you can easily check over version numbers anytime by running docker version.

# docker info

While docker version displays particular version numbers of Docker Engine, docker info steps back and gives some general info and statistics about your Docker container host server. Here you can see a Docker versioning number as well as the current Windows operating system version numbers, along with stats about the server itself such as allocated memory. Probably the most interesting numbers shown in the docker info output are counters for how many containers are on this server, with breakdowns as to how many of those containers are running, paused, or stopped.

# docker --help

This is sort of like issuing docker /?, if that were a real command. The help function of Docker will generate a list of the possible Docker commands that are available to run. This is a good reference point as you get started.

# docker images

After downloading some container images from a repository (we will do this for ourselves in the next section of this chapter), you can use the docker images command to view all of the images that are available on your local system.

# docker search

Utilizing the search function allows you to search the container repositories (such as Docker Hub) for base container images that you might want to utilize in your environment. For example, to search and find images provided from inside Microsoft's Docker Hub repository, issue the following:

```
docker search Microsoft
```