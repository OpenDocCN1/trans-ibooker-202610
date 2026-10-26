```
```cpp
{
    // Add the accumulated subset to the results, indexed by size
    allSubsets[subset.size()].push_back(subset);

    return;
}

// Continue without adding element to subset
GetAllSubsets(set, subset, index + 1, allSubsets);

// Add element to subset
subset.push_back(set[index]);
GetAllSubsets(set, subset, index + 1, allSubsets);
}
```
```

+   5. Returning to our **SubsetSum_BruteForce()** function, we can now declare allSubsets and call the function:

```
```cpp
bool SubsetSum_BruteForce(vector<int> set, int target)
{
    vector<vector<vector<int>>> allSubsets(set.size() + 1);

    GetAllSubsets(set, {}, 0, allSubsets);

    ......
```
```

+   6. Now, we can iterate through each subset and compare its sum to **target**, returning **true** if a match is found:

```
```cpp
for(int size = 0; size <= set.size(); size++)
{
    PRINT("SIZE = " << size << endl);

    for(auto subset : allSubsets[size])
    {
        int sum = 0;

        PRINT("\t{ ");

        for(auto number : subset)
        {
            PRINT(number << " ");

            sum += number;
```
```