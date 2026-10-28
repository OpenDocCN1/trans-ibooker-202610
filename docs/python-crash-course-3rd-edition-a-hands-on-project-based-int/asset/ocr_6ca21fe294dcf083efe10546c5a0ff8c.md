would work the same way if you included those key-value pairs as well. We also print the original value of x_position to see how far the alien moves to the right.

An if-elif-else chain determines how far the alien should move to the right, and assigns this value to the variable x_increment ①. If the alien’s speed is 'slow', it moves one unit to the right; if the speed is 'medium', it moves two units to the right; and if it’s 'fast', it moves three units to the right. Once the increment has been calculated, it’s added to the value of x_position ②, and the result is stored in the dictionary’s x_position.

Because this is a medium-speed alien, its position shifts two units to the right:

```
Original x-position: 0
New x-position: 2
```

This technique is pretty cool: by changing one value in the alien’s dictionary, you can change the overall behavior of the alien. For example, to turn this medium-speed alien into a fast alien, you would add this line:

```
alien_0['speed'] = 'fast'
```

The if-elif-else block would then assign a larger value to x_increment the next time the code runs.

# Removing Key-Value Pairs

When you no longer need a piece of information that’s stored in a dictionary, you can use the del statement to completely remove a key-value pair. All del needs is the name of the dictionary and the key that you want to remove.