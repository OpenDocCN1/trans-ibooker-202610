```
**alien_invasion.py**

```python
def _update_screen(self):
    --snip--
    self.ship.blitme()
    self.aliens.draw(self.screen)

    pygame.display.flip()
```
```

When you call `draw()` on a group, Pygame draws each element in the group at the position defined by its `rect` attribute. The `draw()` method requires one argument: a surface on which to draw the elements from the group. Figure 13-2 shows the first alien on the screen.

![](bbox=[0.13, 0.38, 0.66, 0.66])

Figure 13-2: The first alien appears.

Now that the first alien appears correctly, we’ll write the code to draw an entire fleet.

# # Building the Alien Fleet

To draw a fleet, we need to figure out how to fill the upper portion of the screen with aliens, without overcrowding the