The Content property is used for event-specific data and is where the difference between a SpecialOfferCreated event, a SpecialOfferUpdated, and a SpecialOfferRemoved event appears. Each has its own type of object in Content.

This is all it takes to expose an event feed. This simplicity is the great advantage of using an HTTP-based event feed to publish events. Event-based collaboration can be implemented over a queue system, but that introduces another complex piece of technology that you have to learn to use and administer in production. That complexity is warranted in some situations, but certainly not always.

# CREATING AN EVENT-SUBSCRIBER PROCESS

Subscribing to an event feed essentially means you’ll poll the events endpoint of the microservice you subscribe to. At intervals, you’ll send an HTTP GET request to the “/events” endpoint to check whether there are any events you haven’t processed yet.

We will implement this periodic polling as two main parts:

+   A simple console application that reads one batch of events

+   A Kubernetes CronJob to run the console application at intervals

Putting these two together implements the event subscription: the CronJob makes sure the console application runs at an interval, and each time the console application runs, it sends the HTTP GET request to check whether there are any events to process.

The first step in implementing an event-subscriber process is to create a console application with the following dotnet command:

```
PS> dotnet new console -n EventConsumer
```

and run it with dotnet too:

```
PS> dotnet run
```

The application is empty, so nothing interesting happens yet, but in the next section we will make it read events.

# SUBSCRIBING TO AN EVENT FEED

You now have an EventConsumer console application. All it has to do is read one batch of events and track where the starting point of the next batch of events is. This is done as follows.

# Listing 5.11 Reading a batch of events from an event feed

```
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text.Json;
using System.Threading.Tasks;

var start = await GetStartIdFromDatastore();
var end = 100;
var client = new HttpClient();
```

Read the starting point of this batch from a database.