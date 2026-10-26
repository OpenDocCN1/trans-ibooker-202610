```
```cpp
cin >> u >> v >> w;

edges.push_back(new Edge(u, v, w));
adj[u].push_back(v);
}

vector<int> results;
```
```

3. The first portion of the problem can be solved by using Bellman-Ford in an identical fashion to what was outlined in Exercise 32, *Implementing the Bellman-Ford Algorithm (Part I).* However, instead of printing all the values in the distance array, we will set its return type to `int` and include a few extra lines of code so that it returns only the shortest distance from the source vertex (or `UNKNOWN` if a negative cycle is detected):

```
```cpp
int BellmanFord(int V, int start, vector<Edge*> edges)
{
    // Standard Bellman-Ford implementation

    vector<int> distance(V, UNKNOWN);

    distance[start] = 0;

    for(int i = 0; i < V - 1; i++)
    {
        for(auto edge : edges)
        {
            if(distance[edge->start] == UNKNOWN)
            {
                continue;
            }
            if(distance[edge->start] + edge->weight < distance[edge->end])
            {
                distance[edge->end] = distance[edge->start] + edge->weight;
            }
        }
    }

    // Return UNKNOWN if a negative cycle is found

    if(HasNegativeCycle(distance, edges))
    {
```
```