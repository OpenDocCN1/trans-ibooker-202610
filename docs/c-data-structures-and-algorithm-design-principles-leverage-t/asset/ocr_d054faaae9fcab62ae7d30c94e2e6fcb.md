```
continue;
}

if(dist != UNKNOWN && distance[next] > dist + G.weight[node][next])
{
    distance[next] = distance[node] + G.weight[node][next];
    Q.push({distance[next], next});
}
}
}
return distance;
}
```

10. Now, we will calculate the averages in TestGraph() for each set of paths. We do this by iterating through the array returned by Dijkstra() and keeping a sum of distances for which the index is not equal to the starting node's index. The corresponding value is not equal to UNKNOWN. Every time a valid distance is found, a counter is also incremented so that we can get the final average by dividing the sum by the count. Each one of these averages is then added to the total result, which is divided by the total number of vertices in the graph. Remember that we must reweight the distances again to get the correct values:

```
double result = 0;

for(int i = 0; i < G.V; i++)
{
    vector<int> shortest = Dijkstra(i, G);

    double average = 0;
    int count = 0;

    for(int j = 0; j < G.V; j++)
    {
        if(i == j || shortest[j] == UNKNOWN)
        {
```