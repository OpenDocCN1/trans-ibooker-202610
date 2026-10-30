lowest logical level of granularity that also retains the context in which it was gathered. For observability, that means gathering telemetry at the per request or per service level.

With old-fashioned-style metrics, you had to define custom metrics up front if you wanted to start asking a new question and gathering new telemetry to answer it. In a metrics world, the way to get answers to any possible question is to gather and store every possible metric—which is simply impossible (and, even if it were possible, prohibitively expensive). Further, metrics do not retain the context of the event, simply an aggregate measurement of what occurred at a particular time. With that aggregate view, you could not ask new questions or look for new outliers in the existing data set.

With observability, you can ask new questions of your existing telemetry data at any time, without needing to first predict what those questions might be. In an observable system, you must be able to iteratively explore system aspects ranging from high-level aggregate performance all the way down to the raw data used in individual requests.

The technical requirement making that possible starts with the data format needed for observability: the arbitrarily wide structured event. Collecting these events is not optional. It is not an implementation detail. It is a requirement that makes any level of analysis possible within that wide-ranging view.

# Debugging with Structured Events

First, let’s start by defining an event. For the purpose of understanding the impact to your production services, an *event* is a record of everything that occurred while one particular request interacted with your service.

To create that record, you start by initializing an empty blob right at the beginning, when the request first enters your service. During the lifetime of that request, any interesting details about what occurred—unique IDs, variable values, headers, every parameter passed by the request, the