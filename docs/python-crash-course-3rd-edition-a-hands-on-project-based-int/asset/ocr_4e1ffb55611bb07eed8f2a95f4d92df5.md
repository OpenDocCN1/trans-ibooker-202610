ship. To check when this happens, add a new method in *alien_invasion.py*:

```
***alien_invasion.py***

```python
def _check_aliens_bottom(self):
    """Check if any aliens have reached the bottom of the
screen."""
    for alien in self.aliens.sprites():
        if alien.rect.bottom >= self.settings.screen_hei
    ght:
            # Treat this the same as if the ship got hit.
            self._ship_hit()
            break
```
```

The method *_check_aliens_bottom()* checks whether any aliens have reached the bottom of the screen. An alien reaches the bottom when its *rect.bottom* value is greater than or equal to the screen's height ➊. If an alien reaches the bottom, we call *_ship_hit()*. If one alien hits the bottom, there's no need to check the rest, so we break out of the loop after calling *_ship_hit()*.

We'll call this method from *_update_aliens()*:

```
***alien_invasion.py***

```python
def _update_aliens(self):
    --snip--
    # Look for alien-ship collisions.
    if pygame.sprite.spritecollideany(self.ship, self.ali
ens):
        self._ship_hit()

    # Look for aliens hitting the bottom of the screen.
    self._check_aliens_bottom()
```
```

We call *_check_aliens_bottom()* after updating the positions of all the aliens and after looking for alien-ship collisions. Now a new fleet will appear every time the ship is hit by an alien or an alien reaches the bottom of the screen.