
Unsurprisingly, this is not the preferred way of handling this particular problem, but as with the previous exercises, we can learn quite a bit about how the optimal substructure is formed by implementing recursive solutions like this one. With these insights, we are now equipped to fully understand how the shortest distances between every pair of nodes can be found simultaneously using tabulation.

# All-Pairs Shortest Path

Our program from the previous exercise does print the shortest paths for every vertex pair, but its efficiency is roughly equivalent to performing V calls to Bellman-Ford, with the added memory-related disadvantages associated with recursive algorithms.

Thankfully, there is a very useful bottom-up algorithm for this problem that is equipped to handle everything that the others can in O(V³) time and O(V²) space. It is also quite intuitive, particularly after having implemented the other shortest path algorithms in this book.

# The Floyd-Warshall Algorithm

By now, we should have a fairly clear grasp of how the Bellman-Ford algorithm exploits the optimal substructure that's exhibited in the shortest path problem. The key takeaway is that any shortest path between two graph vertices is going to be a combination of some other shortest path beginning from the source and the edge connecting the path's endpoint to the destination vertex.

The **Floyd-Warshall algorithm** uses this same concept to great effect by making an even broader generalization:

> > "If the shortest distance between Node A and Node B is AB, and the shortest distance between Node B and Node C is BC, then the shortest distance between Node A and Node C is AB + BC."

This logic certainly isn't groundbreaking in and of itself; however, combined with the insight demonstrated by Bellman-Ford—that V iterations across the edges of a graph is sufficient to determine the shortest path from a source node and every other node in a graph—we can use this idea to successively generate the shortest paths between pairs of nodes with Node A as the source, and then use those results to generate potential shortest paths for Node B, C, D, and so on.

# Exercise 44: Implementing the Floyd-Warshall Algorithm

In this exercise, we shall find the shortest distance between every pair of vertices using the Floyd-Warshall algorithm. Let's get started:

+   1. We will begin by including the following headers and defining an UNKNOWN constant:

```
#include <iostream>
#include <vector>
using namespace std;

const int UNKNOWN = 1e9;
```

+   2. Let's begin by handling the input almost exactly like we did in the previous exercise. This time, however, we have no need for an adjacency list representation of the graph:

```
int main()
{
    int V, E;
    cin >> V >> E;

    vector<vector<int>> weight(V, vector<int>(V, UNKNOWN));

    for(int i = 0; i < E; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;

        weight[u][v] = w;
    }
    ....

    return 0;
}
```

+   3. Our FloydWarshall() function will take two arguments—V and weight—and will return a two-dimensional integer vector of shortest-path distances:

```
vector<vector<int>> FloydWarshall(int V, vector<vector<int>> weight)
{
    ....
}
```

```
cout << i << endl;

for(int j = 0; j < V; j++)
{
    cout << "\t" << j << ": ";

    (distance[i][j] == UNKNOWN)
        ? cout << "_" << endl
        : cout << distance[i][j] << endl;
}
}
return 0;
}
```

8. Let's run our program on the following set of input:

Input:

```
7 9
0 1 3
1 2 5
1 3 10
1 5 -4
2 4 2
3 2 -7
4 1 -3
5 6 -8
6 0 12
```

Output:

```
0:
    0: 0
    1: 3
    2: 6
    3: 13
    4: 8
    5: -1
    6: -9
```

```
5: 0
6: -8
```

```
6:
0: 12
1: 15
2: 18
3: 25
4: 20
5: 11
6: 0
```

9. Now, let's try another set of input:

Input:

```
6 8
0 1 3
1 3 -8
2 1 3
2 4 2
2 5 5
3 2 3
4 5 -1
5 1 8
```

Output:

```
NEGATIVE CYCLE FOUND
```

As you can see, Floyd-Warshall is an incredibly useful algorithm that is not only effective but quite easy to implement. In terms of efficiency, whether we should choose Floyd-Warshall or Johnson's algorithm depends entirely on the structure of the graph. But strictly in terms of ease of implementation, Floyd-Warshall is the clear winner.

Once the positions of the houses and roads have been determined, you should produce a new version of the original map according to the following legend:

+   - Houses should be labeled with uppercase letters corresponding to the order they were given in input (that is, 0 = A, 1 = B, 2 = C and so on).
+   - Roads should be indicated with the characters |, -, \, and /, depending on their orientation. If two roads with different orientations intersect, this should be indicated with the + character.
+   - Everything else on the map should be displayed as it was originally given in the input.

# Input Format

The program should take an input in the following format:

+   - The first line contains two space-separated integers, H and W, representing the height and width of the map.
+   - The second contains a single integer, N, which is the number of houses to be built.
+   - The next H lines each contain a string of length W, representing a row on the grid. Valid locations for building roads will be marked with the "." character.
+   - The next N lines contain two integers, x and y, which are the coordinates of the houses. The final index (that is, N - 1) always represents the entry point to the community.

# Output Format

The program should output the same map given in the input, with the following additions:

+   - The positions of each house should be labeled with uppercase letters corresponding to their zero-based index, with the origin on the top left, relative to N (that is, 0 = A, 1 = B, 2 = C, and so on).
+   - The roads connecting each pair of houses should be indicated as follows:
+     - if the road's orientation is horizontal
+     | if the road's orientation is vertical
+     / or \ if the road's orientation is diagonal
+     + if any number of roads with different orientations intersect at the same point

# Hints/Guidelines

+   - To produce the final result, a number of distinct steps are required. It is recommended that you outline the necessary steps prior to their implementation.
+   - It may be quite helpful to devise some scheme for debugging and producing test output for each individual part of the program. A mistake early on in the process is likely to cause subsequent steps to fail.
+   - Study the simpler input and output samples if you are having trouble understanding what needs to be done.
+   - Start by implementing the algorithms you know you will need, particularly the ones we discussed in the previous chapter. There may be multiple ways to accomplish each part of this task–be creative!

# Test Cases

These test cases should help you understand how you need to proceed. Let's begin by taking a simple example:

Input:
5 5
5
.....
.....
.....
.....
.....
0 42 68 35 0
1 70 25 79 59
63 65 0 6 46
82 28 62 92 96
0 43 28 37 0
0 0
4 0
4 4
2 2
0 4

Output:
A...B
.\./.
..D..
./...
E---C

Input:
5 5
5
..#..
.###.
.....
..#..
..##.
0 42 0 35 0
1 0 0 0 59
63 65 0 6 46
82 28 0 92 96
0 43 0 0 0
0 0
4 0
4 4
2 2
0 4

Output:
A.#.B
|###|
|.D.|
|/\| 
E.##C

Figure 9.18: Activity 23, test cases 1 (left) and 2 (right)

Note that the different symbols are used to represent different types of obstacles. Though the effect of any obstacle is the same, we cannot build a road there. Finally, let's step up the complexity in the following example:

# Input:

```
19 25
26
000000000000000000000000000000
0#....................####00
0..&%&. ............####00
0..0##&.##............####0
&&&..&&.&.#.......####.0
##..##.&.#........#0.00
#...#0... ....000.###
##........0000.####
#.........#0##
........000....#000
...................0
...............###.0
..............####000....
..............######......

.0
000..........
0000
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 8 0 50 74 0 59 31 73 45 0 79 24 10 41 66 93 43 0 0 0 0 0 0
0 88 4 0 0 0 28 0 30 41 13 4 70 10 58 61 34 100 79 0 0 0 0 0 0
0 0 0 0 0 0 17 0 0 0 36 98 27 0 13 68 11 34 00 0 0 0 0 0 0
0 0 0 0 0 0 80 0 50 0 80 22 68 73 94 37 86 46 29 0 0 0 0 0
0 0 92 95 0 0 58 0 2 54 0 0 9 45 69 0 91 25 97 0 0 0 31 0 0
0 4 23 67 0 0 50 25 2 54 78 9 29 34 99 82 36 14 0 0 0 66 0 0
0 0 15 64 37 26 0 70 16 95 30 2 18 0 0 96 0 0 0 0 0 0 0 0
0 0 5 52 99 89 24 6 83 53 67 17 38 39 0 0 0 0 45 0 0 0 0
2 98 72 29 38 59 0 78 0 98 95 5 10 0 32 46 76 36 0 0 0 0 0
99 43 100 69 13 61 58 95 9 96 69 14 0 0 0 31 7 63 43 66 0 0 0 0
83 53 68 22 96 13 72 2 91 32 39 58 17 0 91 41 80 36 7 73 99 96 20 0 0
55 24 90 61 6 27 0 24 0 7 14 71 0 0 0 39 0 95 21 45 67 35 27 95 64
```

# Output:

```
000000000000000000000000000000
0#./A--B...C......####00
0../%#&.../....####00
0D0##&...#E...F....####0
666#G&...#\/...####0
##/..&...&.../....#3/00
#/...#|...\/...000/000
#K----L------M0000/####
#F----\_....0000/####
......N.O---P---Q#000
....../....000...####
...../../...R------S0
.../..T.U...###.0.../
..../....####0000
.../....######.../
Z--------V......W
.0........\.....I.....
000........\.....
0000.......X----Y.....
```

# Figure 9.20: Activity 23, test case 5

# Note

The solution to this activity can be found on page 585.
