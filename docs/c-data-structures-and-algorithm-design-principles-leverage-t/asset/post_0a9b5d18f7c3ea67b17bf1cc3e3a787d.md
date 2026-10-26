
```
distance[v] = distance[u] + w;
}
}
}
for(auto edge : edges)
{
    int u = edge->start;
    int v = edge->end;
    int w = edge->weight;

    if(distance[u] == UNKNOWN)
    {
        continue;
    }

    if(distance[u] + w < distance[v])
    {
        cout << "NEGATIVE CYCLE FOUND" << endl;
        return;
    }
}

cout << "DISTANCE FROM VERTEX " << start << ":\n";

for(int i = 0; i < V; i++)
{
    cout << "\t" << i << " : ";

    if(distance[i] == UNKNOWN)
    {
        cout << "Unvisited" << endl;
        continue;
    }
    cout << distance[i] << endl;
}
}
```

4. To test the logic we've added, let's run the algorithm on the following input:

   ```
   6 8
   0 1 3
   1 3 -8
   2 1 3
   2 5 5
   3 2 3
   2 4 2
   4 5 -1
   5 1 8
   0
   ```

5. Our program should output the following:

   ```
   NEGATIVE CYCLE FOUND
   ```

## Activity 15: Greedy Robot

You are developing a pathfinding robot that must find the most efficient path through an obstacle course. For testing purposes, you have designed several courses, each in the shape of a square grid. Your robot is able to traverse any obstacle it encounters, but this also requires a greater expenditure of power. Assuming your robot starts in the top-left corner of the grid and can move in any of the four cardinal directions (north, south, east, and west), you must implement an algorithm that determines the maximum amount of energy your robot can finish the course with.

Since the amount of energy that's required to perform this traversal can be high, you have interspersed power stations throughout the grid, which your robot has the capability of using to recharge itself. Unfortunately, it appears that your robot is quite greedy in terms of energy consumption – if it can reach an energy station multiple times without having to backtrack, it will continually return to the same location until it inevitably overcharges and explodes! Because of this, you will need to predict whether your robot will end up revisiting a power station and abort the traversal attempt before disaster ensues.

### Input

- The first line contains a single integer, N, which is the height and width of the course.
- The next N² - 1 lines each contain the directions string and an integer called power. Each set of N lines corresponds to a single row, beginning from the top of the grid, where each cell's data is defined from left to right (for example, in a 3 x 3 grid, 0 -> [0, 0], 1 -> [0, 1], 2 -> [0, 2], 3 -> [1, 0], 4 -> [1, 1], and so on).
- directions contains 0-3 characters from the set { 'N', 'S', 'E', 'W' }, which represent the cells that your robot can visit from each point. Thus, if the directions string is SW, then the robot can move south or west from that point. power represents the energy expenditure required to cross the cell. Positive values for power indicate that a charging station is located within the cell.

### Output

- If traversing the course causes the robot to explode, print a single line – TRAVERSAL ABORTED.
- Otherwise, print the maximum amount of energy your robot can have upon reaching the bottom-right cell of the course, relative to the amount of energy it started with. For example, if the robot can finish the maze with 10 more units of energy than it started with, print 10; if it finishes the maze with 10 fewer units of energy than it started with, print -10.

### Example

Let's say we had the following input:

```
3
SE -10
SE -8
S -6
S 7
E -10
S 20
E -1
NE 5
```

The grid's layout would look like this:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_325_0.png)

Figure 7.3: Grid for the robot's traversal

The path that reaches the bottom-right cell with the most energy is as follows:

- 0 -> 3 (-10)
- 3 -> 6 (+7)
- 6 -> 7 (-1)
- 7 -> 4 (+5)
- 4 -> 5 (-10)
- 5 -> 8 (+20)

```
(-10) + 7 + (-1) + 5 + (-10) + 20
= 11 more units of energy
```

Therefore, your program should output 11.

### Test Cases

The following test cases should help you understand this problem better:

| Input | Output |
|-------|--------|
| 3<br>SE -10<br>SE -8<br>S -6<br>S 7<br>E -10<br>S 20<br>E -1<br>NE 5 | 11 |

Figure 7.4: Test case 1 for Activity 15

| Input | Output |
|-------|--------|
| 3<br>E -1<br>E -5<br>S 6<br>S -2<br>W 15<br>W -10<br>E -5<br>NE 5 | TRAVERSAL ABORTED |

Figure 7.5: Test case 2 for Activity 15

| Input | Output |
|-------|--------|
| 4<br>S -83<br>E -77<br>SE -93<br>S 86<br>SE -49<br>N -62<br>SE -90<br>S -63<br>S 40<br>NW -72<br>SW -31<br>W 67<br>E -82<br>N -62<br>E -67 | -352 |

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_327_0.png)

| Input | Output |
|-------|--------|
| 5<br>SE -83<br>SE -77<br>E -93<br>S 86<br>W -49<br>E -62<br>SE -90<br>N -63<br>SEW 40<br>NS -72<br>S -11<br>W 67<br>NW -82<br>W -62<br>SW -67<br>S 29<br>W 22<br>SW 69<br>W -93<br>SW -11<br>E 29<br>E -21<br>E -84<br>E -98 | TRAVERSAL ABORTED |

Figure 7.7: Test case 4 for Activity 15

| Input | Output |
|-------|--------|
| 5<br>S 8<br>E -2<br>SEW -15<br>SE 4<br>S 25<br>S -26<br>NEW 19<br>NS 7<br>SEW -56<br>S 11<br>E 25<br>N -86<br>SEW 11<br>NE 26<br>S -78<br>NS -11<br>NSW -76<br>SW 33<br>NSW 4<br>SW -40<br>E 8<br>E 11<br>E 36<br>E -2 | 25 |

Figure 7.8: Test case 5 for Activity 15

### Activity Guidelines

- No algorithms beyond what was covered in Exercise 33, Implementing the Bellman-Ford Algorithm (Part II), are required.
- You may need to reinterpret some of the input so that it corresponds to the actual problem you are trying to solve.
- There is no need to represent the grid as two-dimensional.

> **Note**  
> The solution to this activity can be found on page 537.

We have now established that Bellman-Ford is more versatile than Dijkstra's algorithm since it possesses the capability to produce correct solutions in cases where Dijkstra's algorithm would yield incorrect results. However, if the graph we are considering does not contain any negative edge weights, Dijkstra's algorithm is the obvious choice between the two due to the potentially significant efficiency advantages afforded by its greedy approach. Now, we will explore how Bellman-Ford can be used in conjunction with Dijkstra's algorithm so that it can be used for graphs with negative weights.

## Johnson's Algorithm

Having compared the relative merits and disadvantages of the Bellman-Ford algorithm and Dijkstra's algorithm, we will now discuss an algorithm that combines both of them to retrieve the shortest paths between every pair of vertices in a graph. Johnson's algorithm provides us with the advantage of being able to utilize the efficiency of Dijkstra's algorithm while still producing correct results for graphs with negative edge weights.

The concept behind Johnson's algorithm is quite novel – to contend with Dijkstra's limitations when dealing with negative weights, Johnson's algorithm simply reweights the edges in the graph so they are uniformly non-negative. This is accomplished with the rather creative use of Bellman-Ford combined with some particularly elegant mathematical logic.

The first step in Johnson's algorithm is to add a new 'dummy' vertex to the graph, which is subsequently connected to every other vertex by zero-weighted edges. Bellman-Ford is then used to find the shortest paths between the new vertex and the rest, and the distances are stored for later use.

Consider the implications of the addition of this new vertex: because it has a 0-weighted edge connecting it to every other node in the graph, none of its shortest path distances will ever be positive. Furthermore, its connectivity to every node in the graph ensures that its distance values maintain a constant relation across all the potential traversal paths, which causes the sum that's formed by these values and their corresponding edge weights to 'telescope,' in other words, subsequent terms in the sequence cancel each other out, making the summation equivalent to the difference of the first and last terms. Take a look at the following figure:

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_331_0.png)

Figure 7.9: Applying Johnson's algorithm on a graph with negative weights

In the preceding graph, the diamond-shaped node labeled S represents the dummy vertex, the black parenthesized numbers represent edge weights, the red text represents the shortest paths from S to each node, the orange arrows represent the optimal paths traversed from S, and the blue arrows represent 0-weighted edges branching from S that are not included in any of S's shortest paths.
