```
1     if pygame.sprite.spritecollideany(self.ship, self.aliens):
2         print("Ship hit!!!")
```

The `spritecollideany()` function takes two arguments: a sprite and a group. The function looks for any member of the group that has collided with the sprite and stops looping through the group as soon as it finds one member that has collided with the sprite. Here, it loops through the group `aliens` and returns the first alien it finds that has collided with `ship`.

If no collisions occur, `spritecollideany()` returns None and the if block won’t execute ①. If it finds an alien that has collided with the ship, it returns that alien and the if block executes: it prints Ship hit!!! ②. When an alien hits the ship, we’ll need to do a number of tasks: delete all remaining aliens and bullets, recenter the ship, and create a new fleet. Before we write code to do all this, we want to know that our approach to detecting alien-ship collisions works correctly. Writing a print() call is a simple way to ensure we’re detecting these collisions properly.

Now when you run *Alien Invasion*, the message *Ship hit!!!* should appear in the terminal whenever an alien runs into the ship. When you’re testing this feature, set `fleet_drop_speed` to a higher value, such as 50 or 100, so the aliens reach your ship faster.

# Responding to Alien-Ship Collisions

Now we need to figure out exactly what will happen when an alien collides with the ship. Instead of destroying the `ship` instance and creating a new one, we’ll count how many times the ship has been hit by tracking statistics for the game. Tracking statistics will also be useful for scoring.