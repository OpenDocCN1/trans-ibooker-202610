As you can observe from this graph, the vast majority of requests have low response times, which is great. However, a significant number are over one second and a small number much, much slower, over 4 seconds in fact.

We can quantify the percentage of slow requests using percentiles.
Percentiles give a far richer and more accurate view of response times from a microservice than averages. For example, if we measure response times and calculate percentiles under expected loads, we may get the following:

P50 – 200 milliseconds

P95 – 1200 milliseconds

P99 – 3000 milliseconds

This means that 50% of requests are served in less than 200 milliseconds, 95% are served within 1200 milliseconds, and 99% percent within 3000 milliseconds. These numbers in general look pretty good. But let’s assume our API handles 200 million requests per day (approximately 2314 requests per second). This means 1%, or 2 million requests, take greater than 3 seconds, which is 15 times slower than the 50th percentile - the median. And some requests will be significantly longer than 3 seconds given the long tail response time pattern we see in Figure 9-6.

Long response times are never good things, technically or for client engagement. In fact, many studies have shown how longer response times have negative effects on system usage. For example, the BBC reported that it sees 10% less users for every additional second a page takes to load.¹³ Fast, stable response times are great for business, and one way to achieve this is to reduce the long tail. This also has the effect of decreasing the overall average response time for a service, as the average is skewed heavily by a small number of slow responses.

A common way to eliminate long response times is to fast fail. There are two main ways to achieve this:¹⁴

+   - When a request takes longer than some predefined time limit, instead of waiting for it to complete, the client returns an error to its caller.