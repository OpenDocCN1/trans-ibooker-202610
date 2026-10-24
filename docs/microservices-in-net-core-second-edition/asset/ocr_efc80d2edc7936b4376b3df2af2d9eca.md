```
```
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS final
WORKDIR /app
EXPOSE 80
COPY --from=publish /api/publish ./api
COPY --from=publish /consumer/publish ./consumer
ENTRYPOINT dotnet $STARTUPDLL
```
```

![](bbox=[0.65, 0.11, 0.85, 0.15])

This Dockerfile is a little different from what we have seen before. The Dockerfiles we have seen before have built and published one ASP.NET application. This one builds and publishes an ASP.NET application—the loyalty program—and a .NET console application—the event consumer. The image built with this Dockerfile will therefore have both the LoyaltyProgram.dll and the EventConsumer.dll in it. The last line of the Dockerfile, ENTRYPOINT dotnet $STARTUPDLL, says that when the images built with this Dockerfile start up, they should run dotnet on whatever the environment variable $STARTUPDLL points to. So if $STARTUPDLL is equal to the path to the LoyaltyProgram.dll, the image will run the loyalty program API, and if '$STARTUPDLL' is equal to the path to the EventConsumer.dll, the image will run the loyalty program event consumer.

Let’s see this Dockerfile in action. First we build a loyalty program Docker image:

```
```
> docker build . -t loyalty-program
```
```

Now we run the loyalty program image as either the API part or the event consumer part. Let’s first try to run it as the API by passing in the path to the LoyaltyProgram.dll in the $STARTUPDLL:

```
```
> docker run --rm -p 5001:80 -e STARTUPDLL="api/LoyaltyProgram.dll"
  loyalty-program
```
```

That command also maps port 5001 on your localhost to port 80 in the container. The loyalty program API is now running, and you can call it on localhost port 5001. Let’s run the same Docker image again, but start it up as the Event Consumer:

```
```
> docker run --rm -e STARTUPDLL="consumer/EventConsumer.dll" loyalty-program
```
```

This will run the event consumer, but it will fail to fetch events from the special offers event feed, because we do not have one running. We fix that by running the special offers microservice in a container and putting that on the same Docker network as the event consumer. To do that, we create a new Docker network called “microservices”:

```
```
> docker network create --driver=bridge microservices
```
```

The containers we add to the microservices network can communicate using the container and host names. We will take advantage of that when we run the special offers microservice by adding it to the microservices network and giving it the name special-offers. To do just that, run this command:

```
```
> docker run --rm -p 5002:80 --network=microservices --name=special-offers
  special-offers
```
```