```
11        {
12            _agent = AssignNewAgent();
13        }
14    }
15
16    public int GetUnreadMessagesCount(UserId id)
17    {
18        return _messages.Where(x => x.To == id &&
!x.WasRead).Count();
19    }
20
21    ...
22 }
```

The method checks the ticket’s values to see whether it is escalated and whether the remaining processing time is less than the defined threshold of 50% (line 9). Furthermore, it checks for messages that were not yet read by the current agent (line 10). If all conditions are met, the ticket is requested to be reassigned to a different agent.

The aggregate ensures that all the conditions are checked against strongly consistent data, and it won’t change after the checks are completed by ensuring that all changes to the aggregate’s data are performed as one atomic transaction.

# Referencing other aggregates

Since all objects contained by an aggregate share the same transactional boundary, performance and scalability issues may arise if an aggregate grows too large.

The consistency of the data can be a convenient guiding principle for designing an aggregate’s boundaries. Only the information that is required by the aggregate’s business logic to be strongly consistent should be a part of the aggregate. All information that can be eventually consistent should reside outside of the aggregate’s boundary; for example, as a part of another aggregate, as shown in Figure 6-4.