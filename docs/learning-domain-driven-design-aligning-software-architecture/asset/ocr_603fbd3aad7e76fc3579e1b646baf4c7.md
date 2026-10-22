+   - Reconstitute a state representation—project the events into a state representation that can be used to make business decisions.
+   - Execute the aggregate’s command to execute the business logic, and consequently, produce new domain events.
+   - Commit the new domain events to the event store.

Going back to the example of the Ticket aggregate from Chapter 6, let’s see how it would be implemented as an event-sourced aggregate.

The application service follows the script described earlier: it loads the relevant ticket’s events, rehydrates the aggregate instance, calls the relevant command, and persists changes back to the database:

```
01 public class TicketAPI
02 {
03     private ITicketsRepository _ticketsRepository;
04     ...
05
06     public void RequestEscalation(TicketId id, EscalationReason reason)
07     {
08         var events = _ticketsRepository.LoadEvents(id);
09         var ticket = new Ticket(events);
10         var originalVersion = ticket.Version;
11         var cmd = new RequestEscalation(reason);
12         ticket.Execute(cmd);
13         _ticketsRepository.CommitChanges(ticket, originalVersion);
14     }
15
16     ...
17 }
```

The Ticket aggregate’s rehydration logic in the constructor (lines 27 through 31) instantiates an instance of the state projector class, TicketState, and sequentially calls its AppendEvent method for each of the ticket’s events: