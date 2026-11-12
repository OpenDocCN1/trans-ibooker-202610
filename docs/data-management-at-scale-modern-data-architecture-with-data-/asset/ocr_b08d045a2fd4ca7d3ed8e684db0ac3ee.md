# Notifications Versus Carried State

Events come in different shapes, some of which have more overlap with data distribution and management. One method of distributing events is called event-carried state transfer. With this approach, the event itself carries the full state related to the change, so the consumer has all the information it needs and doesn’t have to make a separate request for the state after being notified of the event. The consumer can even use these events to build up its own local copy of the data. Example 6-1 shows what this might look like for an “add customer” event. An event can also be shaped in such a way that it contains both old and new data, so, for example, for an “update customer” event, consumers can see exactly what information has been changed.

# Example 6-1. Example of an event that carries the full state relating to a customer change

```
{
  "id": "321125215",
  "firstName": "Adolfo",
  "lastName": "Prosacco",
  "gender": "male",
  "birthday": "1997-05-01",
  "address": {
    "streetAddress": "17795 Louisiana 182",
    "city": "Baldwin",
    "state": "Louisiana",
    "postalCode": "70514",
    "country": "United States"
  },
  "phoneNumber": [
    {
      "type": "home",
      "number": "(813) 872-1350"
    },
    {
      "type": "mobile",
      "number": "(202) 555-0154"
    }
  ]
}
```

An alternative approach is to just send a notification about the event, without the full state. In this case, the event will only contain the bare minimum information: a reference to the state that was changed. Although Example 6-2 has been oversimplified, it’s obvious that a consumer of such an event will need to make a call back to the producer to retrieve more information about the change.