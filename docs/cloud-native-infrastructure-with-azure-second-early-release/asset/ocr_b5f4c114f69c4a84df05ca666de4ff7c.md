```
self.send_response(200)
self.end_headers()
self.wfile.write(b"My application with a Summary metric")
LATENCY.observe(time.time() - start_time)

if __name__ == "__main__":
    start_http_server(5555)
    server = http.server.HTTPServer(('localhost', 5551), SampleServer)
    server.serve_forever()
```

To run the preceding code, you can follow the same approach we took in “Counter” on page XX. The metric endpoint at http://localhost:5555/metrics will have two time series:
latency_in_seconds_count and latency_in_seconds_sum.
The former represents the number of observe calls that were made and the latter is the sum of the values passed to observe. If you divide the rate of these two metrics—
rate(latency_in_seconds_count[1m])/rate(latency_in_seconds_sum[1m])—
you will get the average latency for the last minute.

# Histograms

Histograms help you track the size and number of events in buckets while allowing you to aggregate calculations of quantiles. Histograms can be used to measure request durations for a specific HTTP request call.

Code instrumentation for histograms also uses the observe method, and you can combine it with time to track latency, as in our previous example. The following code adds 10 seconds as a request latency:

```
from prometheus_client import Histogram
req_latency = Histogram('request_latency_seconds', 'Description of histogram')
req_latency.observe(10)    # Observe 10 (seconds in this case)
```

Instrumentation is usually applied at the service level or to the client libraries that are being used in the services. It’s important to note that there