| Plays | Quarters | Next play | First plays | Second plays | Third plays |
|---|---|---|---|---|---|
| 1 | 6 | second | 29 | 0 | 8 |

Playing the second slot machine costs one quarter. Because this is the first time this machine has been played since it last paid, not the 100th, the slot machine pays Martha nothing. Martha will play the third slot machine next. This is our new state:

| Plays | Quarters | Next play | First plays | Second plays | Third plays |
|---|---|---|---|---|---|
| 2 | 5 | third | 29 | 1 | 8 |

Playing the third slot machine costs one quarter. Because this is the 9th time this machine has been played since it last paid, not the 10th, the slot machine pays Martha nothing. Next, Martha will cycle back to the first slot machine. This is our new state:

| Plays | Quarters | Next play | First plays | Second plays | Third plays |
|---|---|---|---|---|---|
| 3 | 4 | first | 29 | 1 | 9 |

Now Martha plays the first slot machine:

| Plays | Quarters | Next play | First plays | Second plays | Third plays |
|---|---|---|---|---|---|
| 4 | 3 | second | 30 | 1 | 9 |

Then Martha plays the second slot machine:

| Plays | Quarters | Next play | First plays | Second plays | Third plays |
|---|---|---|---|---|---|
| 5 | 2 | third | 30 | 2 | 9 |