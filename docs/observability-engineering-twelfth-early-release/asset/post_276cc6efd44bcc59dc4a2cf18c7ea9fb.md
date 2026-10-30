
## Query Execution Time

Observability tools need to be fast, with queries ranging from subsecond to low seconds. A key tenet of observability is explorability, since you don’t always know what you’re looking for. You spend less time running the same queries over and over, and more time following a trail of breadcrumbs. During an investigative flow, disruptions when you have to sit and wait for a minute or longer for results can mean you lose your entire train of thought.

In comparison, BI tools are often optimized to run reports, or to craft complex queries that will be used again and again. It’s OK if these take longer to run, because this data isn’t being used to react in real time, but rather to feed into other tools or systems. You typically make decisions about steering the business over time measured in weeks, months, or years, not minutes or seconds. If you’re making strategic business decisions every few seconds, something has gone terribly wrong.

## Accuracy

For observability workloads, if you have to choose, it’s better to return results that are fast as opposed to perfect (as long as they are very close to correct). For iterative investigations, you would almost always rather get a result that scans 99.5% of the events in one second than a result that scans 100% in one minute. This a real and common trade-off that must be made in massively parallelized distributed systems across imperfect and flaky networks (see Chapter 16).

Also (as covered in Chapter 17), some form of dynamic sampling is often employed to achieve observability at scale. Both of these approaches trade a slight bit of accuracy for massive gains in performance. When it comes to BI tools and business data warehouses, both sampling and a “close to right” approach are typically verboten. When it comes to billing, for example, you will always want the accurate result no matter how long it takes.

## Recency

The questions you answer with observability tools have a strong recency bias, and the most important data is often the freshest. A delay of more than a few seconds between when something happened in production and when you can query for those results is unacceptable, especially when you’re dealing with an incident.

As data fades into months past, you tend to care about historical events more in terms of aggregates and trends, rather than granular individual requests. And when you do care about specific requests, taking a bit longer to find them is acceptable. But when data is fresh, you need query results to be raw, rich, and up-to-the-second current.

BI tools typically exist on the other end of that spectrum. It’s generally fine for it to take longer to process data in a BI tool. Often you can increase performance by caching more recent results, or by preprocessing, indexing, or aggregating older data. But generally, with BI tools, you want to retain the full fidelity of the data (practically) forever. With an observability tool, you would never search for something that happened five years ago, or even two years ago. Modern BI data warehouses are designed to store data forever and grow infinitely.

## Structure

Observability data is built from arbitrarily wide and structured data blobs: one event per request per service (or per polling interval in long-running batch processes). For observability needs (to answer any question about what’s happening at any time), you need to append as many event details as possible, to provide as much context as needed, so investigators can spy something that might be relevant in the future. Often those details quickly change and evolve as teams learn what data may be helpful. Defining a data schema up front would defeat that purpose.

Therefore, with observability workloads, schemas must be inferred after the fact or changed on the fly (just start sending a new dimension or stop sending it at any time). Indexes are similarly unhelpful (see Chapter 16).

In comparison, BI tools often collect and process large amounts of unstructured data into structured, queryable form. BI data warehouses would be an ungovernable mess without structures and predefined schemas. You need consistent schemas in order to perform any kind of useful analysis over time. And BI workloads tend to ask similar questions in repeatable ways to power things like dashboards. BI data can be optimized with indexes, compound indexes, summaries, etc.

Because BI data warehouses are designed to grow forever, it is important that they have predefined schemas and grow at a predictable rate. Observability data is designed for rapid feedback loops and flexibility: it is typically most important under times of duress, when predictability is far less important than immediacy.

## Time Windows

Observability and BI tools both have the concept of a session, or trace. But observability tends to be limited to a time span measured in seconds, or minutes at most. BI tools can handle long-term journeys, or traces that can take days or weeks to complete. That type of trace longevity is not a use case typically supported by observability tools. With observability tools, longer-running processes (like import/export jobs or queues) are typically handled with a polling or heartbeat process, and not with a single trace.

## Ephemerality

Summarizing many of these points, debugging data is inherently more ephemeral than business data. You might likely need to retrieve a specific transaction record or billing record from two years ago with total precision, for example. In contrast, you are unlikely to need to know if the latency between service A and service B was high for a particular user request two years ago.

You may, however, want to know if the latency between service A and service B has increased over the last year or two, or if the 95% percentile latency has gone up over that time. That type of question is common, and it is actually best served not by either BI tools/data warehouses or by observability tools. Aggregate historical performance data is best served by our good old pal monitoring (see Chapter 9).

## Using Observability and BI Tools Together in Practice

BI tools come in many forms, often designed to analyze various business metrics that originate from financial, customer relationship management (CRM), enterprise resource planning (ERP), marketing funnel, or supply chain systems. Those metrics are crafted into dashboards and reports by many engineers. But, especially for tech-driven companies, where application service experience is critical, there is often a need to drill further into details that BI systems are designed to provide.

The data granularity for BI tools is arbitrarily large, often reporting metrics per month or per week. Your BI tool can help you understand which feature was most used in the month of February. Your observability tool can provide much more granularity, down to the individual request level. Similar to the way observability and monitoring come together, business intelligence and observability come together by ensuring that you always have micro-level data available that can be used to build macro-level views of the world.

BI tools often lock you into being able to see only an overall super big picture representation of your business world by relying on aggregate metrics. Those views are helpful when visualizing overall business trends, but they’re not as useful when trying to answer questions about product usage or user behavior (detailed earlier in this chapter).

Sharing observability tools across departments is a great way to promote a single-domain language. Observability captures data at a level of abstraction—services and APIs—that can be comprehended by everyone from operations engineers to executives. With that sharing, engineers are encouraged to use business language to describe their domain models, and business people are exposed to the broad (and real) diversity of use cases present in their user base (beyond the few abstract composite user personas they typically see).

## Conclusion

With observability, you can understand customers’ experience of using your software in the real world. Because multiple teams in your business have a vested interest in understanding the customer experience and improving or capitalizing on it, you can use observability to help various teams within your organization better achieve their goals.

Beyond engineering teams, adjacent technical teams like product, support, and customer success can become powerful allies to boost your organization-wide observability adoption initiatives. Less technical teams, like sales and executives, can also become allies with a bit of additional support on your part. The teams outlined in this chapter as examples are by no means definitive. Hopefully, you can use this chapter as a primer to think about the types of business teams that can be better informed and can better achieve desired results by using the observability data you’re capturing. Helping additional business teams achieve their goals will create stakeholders and allies who can, in turn, help prioritize your observability adoption efforts.

In the next chapter, we’ll look at how you can gauge your organization’s overall progress on the observability adoption maturity curve.
