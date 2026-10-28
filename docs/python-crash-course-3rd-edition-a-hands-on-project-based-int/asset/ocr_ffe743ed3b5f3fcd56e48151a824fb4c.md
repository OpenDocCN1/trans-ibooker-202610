![](bbox=[0.12, 0.09, 0.86, 0.48])

Figure 12-3: The ship after firing a series of bullets

# Deleting Old Bullets

At the moment, the bullets disappear when they reach the top, but only because Pygame can’t draw them above the top of the screen. The bullets actually continue to exist; their y-coordinate values just grow increasingly negative. This is a problem because they continue to consume memory and processing power.

We need to get rid of these old bullets, or the game will slow down from doing so much unnecessary work. To do this, we need to detect when the bottom value of a bullet’s rect has a value of 0, which indicates the bullet has passed off the top of the screen:

alien_invasion.py