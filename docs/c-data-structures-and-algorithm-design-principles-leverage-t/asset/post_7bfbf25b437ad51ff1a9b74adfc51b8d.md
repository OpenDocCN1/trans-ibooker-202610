
```cpp
for(int row = 1; row <= maxRow; row++)
{
    int width = (row / 2) + (row % 2);

    vector<int> curr(width + 2);

    for(int col = 1; col <= width; col++)
    {
        curr[col] = prev[col-1] + prev[col];

        if(results.find({row, col}) != results.end())
        {
            queries[{row, col}] = curr[col];
        }
    }
    if(row % 2 == 0)
    {
        curr[width + 1] = curr[width];
    }
    prev = move(curr);
}
for(auto query : queries)
{
    int N = query.first, M = query.second;

    if(M * 2 > N) M = N - M + 1;

    cout << results[{N, M}] << endl;
}
```

In the following table, each row represents a weight, **w**, from 0 to the maximum capacity, and each column represents the index, **i**, of an item. The number in every cell represents the maximum value sum for each weight after the item at index **i** has been considered:

| weight \ index | 0 | 1 | 2 | 3 | 4 |
|---:|---:|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 2 | 0 | 1 | 1 | 1 | 4 |
| 3 | 5 | 5 | 5 | 5 | 5 |
| 4 | 5 | 5 | 6 | 6 | 8 |
| 5 | 9 | 9 | 9 | 9 | 9 |
| 6 | 10 | 10 | 10 | 10 | 12 |
| 7 | 13 | 13 | 13 | 13 | 13 |
| 8 | 14 | 14 | 14 | 14 | 16 |
| 9 | 17 | 17 | 17 | 17 | 17 |
| 10 | 18 | 18 | 18 | 18 | 20 |
| 11 | 19 | 19 | 19 | 19 | 21 |
| 12 | 22 | 22 | 22 | 22 | 24 |

Figure 9.7: Subproblem results for each weight-index pair

As demonstrated in the preceding table, the allowance of duplicates means that no item needs to be excluded as long as its inclusion fits within the maximum capacity. Therefore, whether or not the weight sum could be found at index 0 or index 1,000 of the collection is irrelevant because we are never going to leave a previously found subset sum as is unless adding to it exceeds the defined bounds of the knapsack. This means that there is no advantage to maintaining a record of the item's index, which allows us to cache our subproblems in a single dimension – the combined weight of any number of items encountered. We'll look at its implementation in the following exercise.

## Exercise 42: Unbounded Knapsack

In this exercise, we shall apply the concept of state space reduction to the unbounded knapsack problem by representing our DP table in one dimension. Let's get started:

+   1. Let's use the same headers and input that we used in the previous exercise:

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

.....

int main()
{
    int items, capacity;

    cin >> items >> capacity;

    vector<int> values(items), weight(items);

    for(auto &v : values) cin >> v;
    for(auto &w : weight) cin >> w;

    .....
}
```

2. Now, we will implement a function called UnboundedKnapsack() that returns an integer. Its parameters will be identical to the input:

```
int UnboundedKnapsack(int items, int capacity, vector<int> values,
vector<int> weight)
{
    ....
}
```

3. Our DP table will be represented as an integer vector with size equal to capacity + 1, with each index initialized to 0:

```
vector<int> DP(capacity + 1, 0);
```

4. Like the 0-1 knapsack problem, our state logic will be contained in two nested loops; however, in this variation of the problem, we will invert the nesting of the loops so that the outer loop iterates from 0 to capacity (inclusive), and the inner loop iterates through the item indices:

```
for(int w = 0; w <= capacity; w++)
{
    for(int i = 0; i < items; i++)
    {
        ....
    }
}
```

5. Now, we must decide on how to cache our states. Our only concern is that the capacity is not exceeded by the weights of the chosen items. Since our table is only large enough to represent weight values from 0 to capacity, we only need to make sure that the difference between w and weight[i] is non-negative. Thus, all of the assignment logic can be contained within a single if statement:

```
for(int w = 0; w <= capacity; w++)
{
    for(int i = 0; i < items; i++)
    {
        if(weight[i] <= w)
        {
            DP[w] = max(DP[w], DP[w - weight[i]] + values[i]);
```

```
return DP[capacity];
```

+   6. Now, let's return to **main()**, add a call to **UnboundedKnapsack()**, and output the results:

```
int main()
{
    ...

    int result = UnboundedKnapsack(items, capacity, values, weight);

    cout << "Maximum value of items that can be contained in the knapsack: " << result << endl;

    return 0;
}
```

+   7. Try running your program with the following input:

```
30 335
91 81 86 64 24 61 13 57 60 25 94 54 39 62 5 34 95 12 53 33 53 3 42 75 56 1
84 38 46 62
40 13 4 17 16 35 5 33 35 16 25 29 6 28 12 37 26 27 32 27 7 24 5 28 39 15
38 37 15 40
```

Your output should be as follows:

```
Maximum value of items that can be contained in the knapsack: 7138
```

As demonstrated by the preceding implementation, it is often worth it to consider less costly ways to cache solutions in a DP algorithm. Problems that seem to require complex state representations can often be simplified significantly after closer examination.

## Activity 22: Maximizing Profit

You are working for a large chain of department stores. Like any retail business, your company purchases items from wholesale distributors in large quantities and then sells them at a higher price to gain profit. Certain types of products that are being sold in your store can be purchased from multiple different distributors, but the quality and price of the products can vary considerably, which naturally has an effect on its corresponding retail value. Once factors such as exchange rates and public demand are taken into account, products from certain distributors can often be bought at a much lower price per unit than what they can ultimately be sold for. You have been tasked with designing a system that calculates the maximum profit you can gain with an allotted budget.

You have been provided with a catalog of similar products. Each listed product has the following information:

+   - The wholesale price of the product
+   - The amount of profit that can be made by selling the same product after markup
+   - The quantity of the product sold per unit by the distributor

Given that the distributor will only sell the product in the exact quantity specified, your task is to determine the maximum amount of money that can be made by purchasing some subset of the listed products. To ensure that the store offers a variety of choices, each item that's listed can only be purchased once.

Since you only have a limited amount of warehouse space and don't want to overstock a particular type of item, you are also given a restriction on the maximum number of individual units that can be purchased. Therefore, your program should also ensure that the combined number of products that are bought does not exceed this limit.

## Example

Say five items are listed in the catalog with the following information:

| Product | A | B | C | D | E |
| Quantity | 10 | 5 | 12 | 3 | 4 |
| Price | 20 | 10 | 15 | 50 | 40 |
| Value | 50 | 20 | 20 | 60 | 80 |

Figure 9.8: Sample values for profit optimization

## Test cases

The following set of test cases should help you understand this problem better:

| Input | Output |
|---|---|
| 5 5 5<br>1 1 1<br>1 1 1<br>1 1 1<br>1 1 1<br>1 1 1 | 5 |

Figure 9.9: Activity 22 test case 1

| Input | Output |
|---|---|
| 5 30 25<br>6 11 56<br>13 34 36<br>11 27 31<br>9 31 55<br>11 17 46 | 102 |

Figure 9.10: Activity 22 test case 2

| Input | Output |
|---|---|
| 10 450 50<br>2 66 149<br>19 52 279<br>7 82 474<br>5 96 298<br>7 72 89<br>20 51 573<br>2 36 795<br>7 30 820<br>17 52 155<br>3 81 391 | 3500 |

Figure 9.11: Activity 22 test case 3

However, one problem in graph theory is particularly well suited to the DP approach, and fortunately, it is one we are already very familiar with: the shortest-path problem. In fact, in Chapter 7, *Graph Algorithms II*, we actually discussed an algorithm that's commonly categorized under the DP umbrella, despite the fact that we never identified it as such.

## Reconsidering the Bellman-Ford Algorithm

In our exploration of the Bellman-Ford algorithm, we were viewing it in light of our previous discussions of Dijkstra's algorithm, with which it certainly shares some similarities. But now that we have a solid grasp of the concepts underlying the dynamic programming paradigm, let's reconsider Bellman-Ford according to our new understanding.

In brief, the approach that's used by Bellman-Ford can be described as follows:

Given a source node called **start**, the number of vertices, **V**, and the edges, **E**, of a graph, do the following:

+   1. Mark distances of each node from 0 to **V − 1** (inclusive) as **UNKNOWN**, except for **start**, which is 0.
+   
+   2. Iterate from 1 to **V − 1** (inclusive).
+   
+   3. On each iteration, consider every edge in **E** and check to see whether the source node's respective distance value is **UNKNOWN**. If not, then compare the neighboring node's currently stored distance to the sum of the source node's distance with the edge weight between them.
+   
+   4. If the sum of the source node's distance with the edge weight is less than the destination node's distance, update the destination node's distance to the lesser value.
+   
+   5. After **V − 1** iterations, either the shortest path has been found or the graph has a negative weight cycle, which can be determined with an additional iteration through the edges.
