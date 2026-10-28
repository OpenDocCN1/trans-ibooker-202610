_update_aliens(). I’ll put these new methods after _create_alien(), but again, the placement of these methods in the class isn’t critical.

# **alien_invasion.py**

```
```python
def _check_fleet_edges(self):
    """Respond appropriately if any aliens have reached an edge."""
    for alien in self.aliens.sprites():
        if alien.check_edges():
            self._change_fleet_direction()
            break

def _change_fleet_direction(self):
    """Drop the entire fleet and change the fleet's direction."""
    for alien in self.aliens.sprites():
        alien.rect.y += self.settings.fleet_drop_speed
    self.settings.fleet_direction *= -1
```
```

In _check_fleet_edges(), we loop through the fleet and call check_edges() on each alien ❶. If check_edges() returns True, we know an alien is at an edge and the whole fleet needs to change direction; so we call _change_fleet_direction() and break out of the loop ❷. In _change_fleet_direction(), we loop through all the aliens and drop each one using the setting fleet_drop_speed ❸; then we change the value of fleet_direction by multiplying its current value by −1. The line that changes the fleet’s direction isn’t part of the for loop. We want to change each alien’s vertical position, but we only want to change the direction of the fleet once.

Here are the changes to _update_aliens():

# **alien_invasion.py**

```
```python
def _update_aliens(self):
    """Check if the fleet is at an edge, then update positions."""
```
```