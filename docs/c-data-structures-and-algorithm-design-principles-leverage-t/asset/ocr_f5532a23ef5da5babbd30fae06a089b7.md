15. To store the shortest path distances for every pair of nodes, we initialize a matrix with V rows (so that each pair of two-dimensional indices, [i, j], represents the shortest path between vertex i and vertex j). We then perform V calls to Dijkstra's algorithm, which returns the distance array for each starting node:

```
```cpp
// Create a matrix for storing distance values
vector<vector<int>> shortest(V);

// Retrieve shortest distances for each vertex
for(int i = 0; i < V; i++)
{
    shortest[i] = Dijkstra(V, i, edges);
}
```
```

16. Unsurprisingly, the results we have accumulated in this step are quite inaccurate. Every distance value is now positive as a result of our reweighting operation. However, this can be rectified quite simply by applying the same formula to each result in reverse:

```
```cpp
// Reweight again in reverse to get original values
for(int i = 0; i < V; i++)
{
    cout << i << ":\n";

    for(int j = 0; j < V; j++)
    {
        if(shortest[i][j] != UNKNOWN)
        {
            shortest[i][j] += h[j] - h[i];

            cout << "\t" << j << ": " << shortest[i][j] << endl;
        }
    }
}
```
```

17. Now, let's return to our **main()** function and implement the code for handling input. After we have collected the edges of the input graph, we simply need to perform a single call to **Johnson()** and our work is done:

```
```cpp
int main()
{
    int V, E;
    cin >> V >> E;
```
```