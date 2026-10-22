```
public void Apply(OrderSubmitted @event)
{
    Version += 1;
}

public void Apply(PaymentConfirmed @event)
{
    Version += 1;
}
}
```

Iterating an aggregate’s events and feeding them sequentially into the appropriate overrides of the Apply method will produce precisely the state representation modeled in the table in Table 7-1.

Pay attention to the Version field that is incremented after applying each event. Its value represents the total number of modifications made to the business entity. Moreover, suppose we apply a subset of events. In that case, we can “travel through time”: we can project the entity’s state at any point of its lifecycle by applying only the relevant events. For example, if we need the entity’s state in version 5, we can apply only the first five events.

Finally, we are not limited to projecting only a single state representation of the events! Consider the following scenarios.

# Search

You have to implement a search. However, since a lead’s contact information can be updated—first name, last name, and phone number—sales agents may not be aware of the changes applied by other agents and may want to locate leads using their contact information, including historical values. We can easily project the historical information:

```
public class LeadSearchModelProjection
{
    public long LeadId { get; private set; }
```