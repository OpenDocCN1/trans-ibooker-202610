```
visited[curr] = true;

// Iterate through edges
for(auto edge : edges)
{
    // Only consider neighboring nodes
    if(edge->start != curr) continue;

    // Disregard if already visited
    if(visited[edge->end]) continue;

    if(distance[curr] != UNKNOWN && distance[curr] + edge->weight < distance[edge->end])
    {
        distance[edge->end] = distance[curr] + edge->weight;
    }
}

return distance;
```

11. We now have everything we need to perform Johnson's algorithm. Let's declare a new function, Johnson(), which also takes V and edges as arguments:

```
void Johnson(int V, vector<Edge*> edges)
```

12. We start by creating an integer vector, h, and setting it to the output of BellmanFord():

```
// Get distance array from modified graph
vector<int> h = BellmanFord(V, edges);
```

13. We check whether h is empty. If it is, we terminate the function:

```
if(h.empty()) return;
```

14. Otherwise, we apply the reweighting formula:

```
for(int i = 0; i < edges.size(); i++)
{
    edges[i]->weight += (h[edges[i]->start] - h[edges[i]->end]);
}
```