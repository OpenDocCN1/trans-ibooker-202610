is actually best served not by either BI tools/data warehouses or by observability tools. Aggregate historical performance data is best served by our good old pal monitoring (see [Chapter 9](#)).

# Using Observability and BI Tools Together in Practice

BI tools come in many forms, often designed to analyze various business metrics that originate from financial, customer relationship management (CRM), enterprise resource planning (ERP), marketing funnel, or supply chain systems. Those metrics are crafted into dashboards and reports by many engineers. But, especially for tech-driven companies, where application service experience is critical, there is often a need to drill further into details that BI systems are designed to provide.

The data granularity for BI tools is arbitrarily large, often reporting metrics per month or per week. Your BI tool can help you understand which feature was most used in the month of February. Your observability tool can provide much more granularity, down to the individual request level. Similar to the way observability and monitoring come together, business intelligence and observability come together by ensuring that you always have micro-level data available that can be used to build macro-level views of the world.

BI tools often lock you into being able to see only an overall super big picture representation of your business world by relying on aggregate metrics. Those views are helpful when visualizing overall business trends, but they’re not as useful when trying to answer questions about product usage or user behavior (detailed earlier in this chapter).

Sharing observability tools across departments is a great way to promote a single-domain language. Observability captures data at a level of abstraction—services and APIs—that can be comprehended by everyone from operations engineers to executives. With that sharing, engineers are encouraged to use business language to describe their domain models, and