```
if guess == 0:
    print('Exiting game.')
    break
elif guess == n:
    print('Success! You win.')
    break
elif guess < n:
    print('Too low.', end=' ')
else:
    print('Too high.', end=' ')

while True:
    play_the_game()
    ans = input('Want to play again? (Y or N): ')
    if not ans or ans[0] in 'Nn':
        break
```

# # 11.5 CREATING A DECK OBJECT

The **shuffle** function is one of the most useful in the **random** package. This function, as you might guess, is especially useful for simulating a deck of cards, and you’d be right. But it’s extensible to other situations as well.

The action of **shuffle** is to rearrange the order of a list so that any element can appear in any position. The number of elements does not change, nor do the number of duplicate items, if any. So, for example, suppose you shuffle the following list:

Click here to view code image

```
kings_list = ['John', 'James', 'Henry', 'Henry',
 'George']
```