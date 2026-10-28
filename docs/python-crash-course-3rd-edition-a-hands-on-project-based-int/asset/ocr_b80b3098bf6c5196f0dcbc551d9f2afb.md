```
```python
score_str = str(self.stats.score)
self.score_image = self.font.render(score_str, True,
        self.text_color, self.settings.bg_color)

# Display the score at the top right of the screen.
self.score_rect = self.score_image.get_rect()
self.score_rect.right = self.screen_rect.right - 20
self.score_rect.top = 20
```
```

In prep_score(), we turn the numerical value stats.score
into a string ❶ and then pass this string to render(), which
creates the image ❷. To display the score clearly onscreen,
we pass the screen's background color and the text color to
render().

We'll position the score in the upper-right corner of the
screen and have it expand to the left as the score increases
and the width of the number grows. To make sure the score
always lines up with the right side of the screen, we create a
rect called score_rect ❸ and set its right edge 20 pixels from
the right edge of the screen ❹. We then place the top edge
20 pixels down from the top of the screen ❺.

Then we create a show_score() method to display the
rendered score image:

**scoreboard.py**

```
```python
def show_score(self):
    """Draw score to the screen."""
    self.screen.blit(self.score_image, self.score_rect)
```
```

This method draws the score image onscreen at the
location score_rect specifies.

# ## **Making a Scoreboard**

To display the score, we'll create a Scoreboard instance in
AlienInvasion. First, let's update the import statements: