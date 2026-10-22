```
);

_events.Append(confirmed);
_events.Append(routeGenerated);
_events.Append(commandIssuedEvent);
}

public void Process(FlightBooked booked)
{
    var commandIssuedEvent = new CommandIssuedEvent(
        command: new BookHotel(_destination, _parameters)
    );

    _events.Append(booked);
    _events.Append(commandIssuedEvent);
}

... 
}
```

In this example, the process manager has its explicit ID and persistent state, describing the trip that has to be booked. As in the earlier example of a saga pattern, the process manager subscribes to events that control the workflow (RouteConfirmed, RouteRejected, ReroutingConfirmed, etc.), and it instantiates events of type Command IssuedEvent that will be processed by an outbox relay to execute the actual commands.

# Conclusion

In this chapter, you learned the different patterns for integrating a system’s components. The chapter began by exploring patterns for model translations that can be used to implement anticorruption layers or open-host services. We saw that translations can be handled on the fly or can follow a more complex logic, requiring state tracking.

The outbox pattern is a reliable way to publish aggregates’ domain events. It ensures that domain events are always going to be