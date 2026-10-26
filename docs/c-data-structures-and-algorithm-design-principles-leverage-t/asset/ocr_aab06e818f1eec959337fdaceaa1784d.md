5. This time, let's move the final check for negative cycles into its own function:

```
bool HasNegativeCycle(vector<int> distance, vector<Edge*> edges)
{
    for(auto edge : edges)
    {
        int u = edge->start;
        int v = edge->end;
        int w = edge->weight;

        if(distance[u] == UNKNOWN) continue;

        if(distance[u] + w < distance[v])
        {
            return true;
        }
    }
    return false;
}
```

6. Now, we can call it at the end of the original function and return an empty array if a negative cycle is found:

```
if(HasNegativeCycle(distance, edges))
{
    cout << "NEGATIVE CYCLE FOUND" << endl;

    return {};
}
```

7. After ensuring that the graph has no negative cycles, we can return the resultant set of distance values to the calling function and apply the reweighting formula to every edge in the graph. But first, let’s implement Dijkstra's algorithm:

```
vector<int> Dijkstra(int V, int start, vector<Edge*> edges)
```