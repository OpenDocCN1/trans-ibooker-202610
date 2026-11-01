stacktrace: stacktrace

Finished in 0.04 seconds
1 property, 1 failure

That looks a little bit different than our usual ExUnit tests. stream_data adds the information in the highlighted lines to regular ExUnit errors. This section includes information about the generated data. The first line tells us how many terms stream_data generated before finding the failure. In this case, it successfully generated three lists before finding one that would fail. After that, stream_data reports the values that were generated for each clause in the check all (we have only one clause here). This is more useful when there are many clauses since it helps retrace the steps of what pieces of data were generated.

We expected that our bad sorting function would fail for any list that wasn’t already sorted. That’s exactly what happened: as soon as we got a simple not-sorted list, [1, 0], the test failed. On second thought, isn’t it a bit suspicious that the list that caused the failure is the simplest possible list that causes this particular failure? This suspicion would be confirmed by running the property again. If we run the property a few times, [1, 0] is going to be the failing list most of the time. Alternative results might look similar: for example, [0, -1] or [2, 1]. To better understand what’s going on, we could print the generated list right as the first line in the do/end block of the check all.

```
> mix test

[]
[-1]
[1, 2]
[1, 1, 0]
» [1, 0]
» [0]
» [1]
```

rest of the test result