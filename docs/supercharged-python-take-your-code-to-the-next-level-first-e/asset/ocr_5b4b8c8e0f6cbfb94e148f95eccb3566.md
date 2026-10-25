![](bbox=[0.158, 0.089, 0.814, 0.358])

Figure 11.6. How three guesses are needed at N = 7

We can now determine the maximum number of guesses required at certain numbers. When N falls between these numbers, we round upward to the next step—because to get the maximum number of guesses needed, we must assume a worst- case scenario. Table 11.8 shows the progression.

# Table 11.8. Maximum Number of Guesses Required by the Game

| Size = N | N + 1 | Number of guesses required = log2(N+1) |
|----------|-------|----------------------------------------|
| 1        | 2     | 1                                      |
| 3        | 4     | 2                                      |
| 7        | 8     | 3                                      |
| 15       | 16    | 4                                      |