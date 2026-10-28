```
```python
def update(self):
    """Update the ship's position based on movement flags."""

    # Update the ship's x value, not the rect.
    if self.moving_right:
        self.x += self.settings.ship_speed
    if self.moving_left:
        self.x -= self.settings.ship_speed

    # Update rect object from self.x.
    self.rect.x = self.x

def blitme(self):
    --snip--
```
```

We create a `settings` attribute for `Ship`, so we can use it in `update()` ❶. Because we’re adjusting the position of the ship by fractions of a pixel, we need to assign the position to a variable that can have a float assigned to it. You can use a float to set an attribute of a rect, but the rect will only keep the integer portion of that value. To keep track of the ship’s position accurately, we define a new `self.x` ❷. We use the `float()` function to convert the value of `self.rect.x` to a float and assign this value to `self.x`.

Now when we change the ship’s position in `update()`, the value of `self.x` is adjusted by the amount stored in `settings.ship_speed` ❸. After `self.x` has been updated, we use the new value to update `self.rect.x`, which controls the position of the ship ❹. Only the integer portion of `self.x` will be assigned to `self.rect.x`, but that’s fine for displaying the ship.

Now we can change the value of `ship_speed`, and any value greater than 1 will make the ship move faster. This will help make the ship respond quickly enough to shoot down aliens, and it will let us change the tempo of the game as the player progresses in gameplay.