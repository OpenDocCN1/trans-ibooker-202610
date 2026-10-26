11. In `main()`, V and adj would be defined as follows:

```
int V = 9;

vector<vector<int>> adj =
{
    { 1, 3 },
    { 2, 4 },
    { 3, 5 },
    { 7 },
    { 2 },
    { 4, 6 },
    { 7, 2 },
    { 8 },
    { 3 }
};
```

12. Upon executing our program, the following output should be displayed:

```
Graph contains 4 strongly connected components.
0
1
2 4 5 6
3 8 7
```

# Activity 17: Maze-Teleportation Game

You are designing a game where multiple players are placed randomly in a maze of rooms. Each room contains one or more teleportation devices that the players can use to travel between different parts of the maze. Every teleporter has a value associated with it, which is added to any player's score who uses it. Players alternately take turns traversing the maze until every room has been visited at least once, at which point the round ends and the player with the lowest score is the winner.

You have implemented a system that procedurally generates a new maze at the beginning of every game. Unfortunately, you recently discovered that some of the generated mazes contained loops that a player could use to endlessly reduce their score. You also noticed that players frequently had an unfair advantage, depending on the room they started in. Worst of all, the teleporters are often dispersed in such a way that a player can end up being cut off from the rest of the maze for the duration of the round.