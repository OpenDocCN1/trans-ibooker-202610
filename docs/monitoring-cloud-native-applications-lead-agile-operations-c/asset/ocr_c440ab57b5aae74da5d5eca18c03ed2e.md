# Line Graphs

Line graphs are the most common and often the default option for converting a metric into a graph. A line graph represents a trendline by joining the various data points collected across different time intervals over a selected time range. For example, if the metrics are collected every 10 seconds for server requests reaching four different web servers, the trendline for each of them would join the data points received at a regular interval of 10 seconds over a 1-hour time range, would look similar to the line graph shown in Figure 3-15:

![](bbox=[0.068, 0.321, 0.845, 0.459])

Figure 3-15. Line graph of server requests to four web servers

On close inspection, we can see that the line graph for one of the web servers, web_server_04, has dropped significantly at 11:05, which might mean that it was down at the time and was unable to serve requests. Since it is difficult to clearly see what is going on if the number of hosts is higher, we can isolate only the one web server we are interested in, in this case, web_server_04. The line graph for web_server_04 would look like the graph shown in Figure 3-16:

![](bbox=[0.068, 0.668, 0.845, 0.806])

Figure 3-16. Line graph of server requests to web_server_04