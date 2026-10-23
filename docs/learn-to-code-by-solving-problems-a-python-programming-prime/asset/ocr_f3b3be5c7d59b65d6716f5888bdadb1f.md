```
print(f'Player {player} scores {points} point(s).')

if player == 'A':
    score_a = score_a + points
    player = 'B'
else:
    score_b = score_b + points
    player = 'A'

print(f'Player A: {score_a} point(s).')
print(f'Player B: {score_b} point(s).')
```

# Listing 6-1: Solving Card Game

I’ve introduced the constant NUM_CARDS to refer to 52 ❶. We’ll use it a couple of times in the code, and it’s easier to remember what NUM_CARDS means than what 52 means.

Next we define the no_high function, including docstring, that we’ve discussed in depth ❷. We’ll always put our functions near the top of our programs. This way, the functions are available to be called by any code that follows them.

The main part of the program starts with creating a list that will hold the cards in the deck ❸. We then read the cards from the input ❹, appending each to the deck. You’ll notice that cards are never literally removed or taken from the deck (the deck remains as is throughout program execution). We could have done it that way. Instead, I’ve chosen to track where we are in the deck so that we know which card would be removed next.

There are three other crucial variables that we maintain: score_a, the current total score for player A; score_b, the current total score for player B; and player, the name of the current player.

Our next task is to look at each card in the deck to give points to the players. A regular for loop would let us look at the current card. But that’s not enough: if the current card is a high card, then we have to be able to look at later cards as well. To facilitate that, we use a range for loop ❺.

On each iteration of this loop, we determine the number of points awarded to the current player based on the card that they take from the deck. Each rule for getting points depends on the deck having some number of remaining cards. The remaining variable ❻ tells us the number of remaining cards. When i is 0, the number of remaining cards is 51, because we’ve just taken the first card. When i is 1, the number of