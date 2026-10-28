
Ship类控制飞船的所有属性，因此我们为它添加一个名为moving_right的属性，以及一个update()方法来检查moving_right标志的状态。如果该标志设置为True，update()方法将改变飞船的位置。我们将在每次通过while循环时调用这个方法，以更新飞船的位置。

以下是Ship类的更改：

ship.py

```python
class Ship:
    """A class to manage the ship."""

    def __init__(self, ai_game):
        --snip--
        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

        # Movement flag; start with a ship that's not moving.
        self.moving_right = False

    def update(self):
        """Update the ship's position based on the movement flag."""
        if self.moving_right:
            self.rect.x += 1

    def blitme(self):
        --snip--
```

我们在__init__()方法中添加了一个self.moving_right属性，并将其初始设置为False ❶。然后添加update()，如果标志为True ❷，它将飞船向右移动。update()方法将从类外部调用，因此它不被视为辅助方法。

现在我们需要修改_check_events()，使得当按下右箭头键时moving_right设为True，当松开按键时设为False：

```python
def _check_events(self):
    """Respond to keypresses and mouse events."""
    for event in pygame.event.get():
        --snip--
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                self.ship.moving_right = True
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_RIGHT:
                self.ship.moving_right = False
```

这里，我们修改了游戏对玩家按下右箭头键时的响应方式：而不是直接改变飞船的位置，我们只需将`moving_right`设置为`True` ❶。然后我们添加一个新的`elif`块，它响应KEYUP事件 ❷。当玩家松开右箭头键（`K_RIGHT`）时，我们将`moving_right`设置为`False`。

接下来，我们修改`run_game()`中的`while`循环，使它在每次循环中调用飞船的`update()`方法：

```python
def run_game(self):
    """Start the main loop for the game."""
    while True:
        self._check_events()
        self.ship.update()
        self._update_screen()
        self.clock.tick(60)
```

飞船的位置将在我们检查键盘事件之后、更新屏幕之前进行更新。这允许飞船的位置根据玩家的输入进行更新，并确保在将飞船绘制到屏幕时使用更新后的位置。

当你运行*alien_invasion.py*并按住右箭头键时，飞船应该持续向右移动，直到你松开按键。

### 左右移动

现在飞船可以持续向右移动了，添加向左移动就很简单了。我们再次修改`Ship`类和`_check_events()`方法。以下是`Ship`中`__init__()`和`update()`的相关更改：

ship.py

```python
def __init__(self, ai_game):
    --snip--
    # Movement flags; start with a ship that's not moving.
    self.moving_right = False
    self.moving_left = False

def update(self):
    """Update the ship's position based on movement flags."""
    if self.moving_right:
        self.rect.x += 1
    if self.moving_left:
        self.rect.x -= 1
```

在`__init__()`中，我们添加了一个`self.moving_left`标志。在`update()`中，我们使用两个独立的`if`块，而不是`elif`，以允许当两个箭头键都被按住时，飞船的`rect.x`值先增加再减少。这会导致飞船静止不动。如果我们对向左移动使用`elif`，右箭头键将始终具有优先级。使用两个`if`块可以在玩家改变方向时可能短暂同时按住两个键的情况下，使移动更加准确。

我们需要对`_check_events()`做出两处添加：

alien_invasion.py

```python
def _check_events(self):
    """Respond to keypresses and mouse events."""
    for event in pygame.event.get():
        --snip--
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                self.ship.moving_right = True
            elif event.key == pygame.K_LEFT:
                self.ship.moving_left = True
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_RIGHT:
                self.ship.moving_right = False
            elif event.key == pygame.K_LEFT:
                self.ship.moving_left = False
```

如果发生针对K_LEFT键的KEYDOWN事件，我们将`moving_left`设置为True。如果发生针对K_LEFT键的KEYUP事件，我们将`moving_left`设置为False。这里我们可以使用`elif`块，因为每个事件只与一个键相关。如果玩家同时按下两个键，将检测到两个独立的事件。

现在当你运行*alien_invasion.py*时，你应该能够持续向右和向左移动飞船。如果你同时按住两个键，飞船应该停止移动。

接下来，我们将进一步优化飞船的移动。让我们调整飞船的速度，并限制飞船可以移动的范围，使其不会从屏幕两侧消失。

### 调整飞船速度

目前，飞船在`while`循环的每次循环中移动一个像素，但我们可以通过在Settings类中添加一个`ship_speed`属性来更精细地控制飞船的速度。我们将使用这个属性来确定每次经过循环时飞船移动的距离。以下是*settings.py*中的新属性：

settings.py

```python
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        --snip--

        # Ship settings
        self.ship_speed = 1.5
```

我们将`ship_speed`的初始值设置为1.5。现在当飞船移动时，它的位置在每次循环中会调整1.5像素（而不是1像素）。

我们使用浮点数作为速度设置，以便在后续加快游戏节奏时能更精细地控制飞船的速度。然而，`rect`属性（如x）只存储整数值，因此我们需要对`Ship`做出一些修改：

ship.py

```python
class Ship:
    """A class to manage the ship."""

    def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
        self.screen = ai_game.screen
        self.settings = ai_game.settings
        --snip--

        # Start each new ship at the bottom center of the screen.
        self.rect.midbottom = self.screen_rect.midbottom

        # Store a float for the ship's exact horizontal position.
        self.x = float(self.rect.x)

        # Movement flags; start with a ship that's not moving.
        self.moving_right = False
        self.moving_left = False

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

我们为`Ship`创建了一个`settings`属性，以便在`update()`中使用它 ❶。由于我们是以像素的几分之一来调整飞船的位置，所以需要将位置赋值给一个可以存储浮点数的变量。你可以使用浮点数设置rect的属性，但rect只会保留该值的整数部分。为了准确跟踪飞船的位置，我们定义了一个新的`self.x` ❷。我们使用`float()`函数将`self.rect.x`的值转换为浮点数，并将该值赋给`self.x`。

现在当我们在`update()`中改变飞船的位置时，`self.x`的值会根据`settings.ship_speed`中存储的量进行调整 ❸。在`self.x`更新后，我们使用新值来更新`self.rect.x`，它控制飞船的位置 ❹。只有`self.x`的整数部分会被赋值给`self.rect.x`，但这对于显示飞船来说已经足够了。

现在我们可以更改`ship_speed`的值，任何大于1的值都会使飞船移动得更快。这将有助于飞船快速响应以击落外星人，并且让我们能够在玩家游戏过程中改变游戏的节奏。

