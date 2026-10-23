7. The output may resemble the following. The report has a start time and end time spanning five days. The maximum is 30 days, and data exists for only a rolling 30-day period.

```
AggregationLevel  : State
ProviderLocation  :
{
  "Country": "United States",
  "State": "minnesota"
}
ReachabilityReport :
[
{
  "Provider": "Comcast Cable Communications, LLC - ASN 7922",
  "AzureLocation": "Central US",
  "Latencies": [
    {
      "TimeStamp": "2019-05-29T00:00:00Z",
      "Score": 96
    },
    {
      "TimeStamp": "2019-05-28T00:00:00Z",
      "Score": 94
    },
    {
      "TimeStamp": "2019-05-27T00:00:00Z",
      "Score": 98
    },
    {
      "TimeStamp": "2019-05-26T00:00:00Z",
      "Score": 98
    },
    {
      "TimeStamp": "2019-05-25T00:00:00Z",
      "Score": 98
    }]
}
]
```