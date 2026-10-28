

**alien_invasion.py**

```python
def _check_bullet_alien_collisions(self):
    --snip--
    if not self.aliens:
        # Destroy existing bullets and create new fleet.
        self.bullets.empty()
        self._create_fleet()
        self.settings.increase_speed()
```

更改速度设置 ship_speed、alien_speed 和 bullet_speed 的值足以加快整个游戏的速度！

### 重置速度

现在我们需要在玩家每次开始新游戏时将所有更改过的设置恢复到初始值；否则，每次新游戏都会以上一局增加后的速度设置开始：

**alien_invasion.py**

```python
def _check_play_button(self, mouse_pos):
    """Start a new game when the player clicks Play."""
    button_clicked = self.play_button.rect.collidepoint(mouse_pos)
    if button_clicked and not self.game_active:
        # Reset the game settings.
        self.settings.initialize_dynamic_settings()
        --snip--
```

玩 *Alien Invasion* 现在应该更有趣也更具挑战性了。每次你清除屏幕时，游戏都会加速并变得稍微更困难一些。如果游戏变得太快太难，就降低 settings.speedup_scale 的值。或者如果游戏挑战性不够，就稍微增加该值。通过在合理的时间内逐步提升难度来找到一个合适的平衡点。前几个屏幕应该很简单，接下来的几个应该有挑战性但可以完成，而之后的屏幕应该几乎不可能完成。

## TRY IT YOURSELF

- **14-3. Challenging Target Practice:** 在练习 14-2（第 283 页）的基础上进行工作。随着游戏的进行使目标移动得更快，并在玩家点击 Play 时以原始速度重新开始目标。
- **14-4. Difficulty Levels:** 为 *Alien Invasion* 制作一组按钮，让玩家可以选择合适的游戏起始难度级别。每个按钮都应为 `Settings` 中创建不同难度级别所需的属性分配适当的值。

## Scoring

让我们实现一个计分系统，以实时跟踪游戏分数并显示最高分、等级和剩余飞船数量。

分数是游戏统计数据，因此我们将在 `GameStats` 中添加一个 `score` 属性：

**game_stats.py**

```python
class GameStats:
    --snip--
    def reset_stats(self):
        """Initialize statistics that can change during the game."""
        self.ships_left = self.ai_settings.ship_limit
        self.score = 0
```

为了在每次新游戏开始时重置分数，我们在 `reset_stats()` 中初始化 `score`，而不是在 `__init__()` 中。

### Displaying the Score

```python
score_str = str(self.stats.score)
self.score_image = self.font.render(score_str, True,
        self.text_color, self.settings.bg_color)

# Display the score at the top right of the screen.
self.score_rect = self.score_image.get_rect()
self.score_rect.right = self.screen_rect.right - 20
self.score_rect.top = 20
```

在 prep_score() 中，我们将数值 stats.score 转换为字符串 ❶，然后将此字符串传递给 render()，它创建图像 ❷。为了在屏幕上清晰地显示分数，我们将屏幕的背景颜色和文本颜色传递给 render()。

我们将分数定位在屏幕右上角，并使其随着分数增加和数字宽度变大而向左扩展。为了确保分数始终与屏幕右侧对齐，我们创建一个名为 score_rect 的 rect ❸，并将其右边缘设置在屏幕右边缘右侧 20 像素处 ❹。然后我们将顶部边缘放置在屏幕顶部下方 20 像素处 ❺。

然后我们创建一个 show_score() 方法来显示渲染后的分数图像：

**scoreboard.py**

```python
def show_score(self):
    """Draw score to the screen."""
    self.screen.blit(self.score_image, self.score_rect)
```

此方法将分数图像绘制到屏幕上，位置由 score_rect 指定。

### Making a Scoreboard

为了显示分数，我们将在 AlienInvasion 中创建一个 Scoreboard 实例。首先，让我们更新导入语句：

**alien_invasion.py**

```python
--snip--
from game_stats import GameStats
from scoreboard import Scoreboard
--snip--
```

接下来，我们在 __init__() 中创建 Scoreboard 的实例：

**alien_invasion.py**

```python
def __init__(self):
    --snip--
    pygame.display.set_caption("Alien Invasion")

    # Create an instance to store game statistics,
    #   and create a scoreboard.
    self.stats = GameStats(self)
    self.sb = Scoreboard(self)
    --snip--
```

然后我们在 _update_screen() 中在屏幕上绘制记分板：

**alien_invasion.py**

```python
def _update_screen(self):
    --snip--
    self.aliens.draw(self.screen)

    # Draw the score information.
    self.sb.show_score()

    # Draw the play button if the game is inactive.
    --snip--
```

我们在绘制 Play 按钮之前调用 show_score()。

现在运行 Alien Invasion 时，屏幕右上角应该会出现一个 0。（此时，我们只想确保分数出现在正确的位置，然后再进一步开发计分系统。）图 14-2 显示了游戏开始前分数的显示情况。

接下来，我们将为每个外星人分配分数值！

![](img/8f15563c16d2a98c4d3115150679b092_472_0.png)

Figure 14-2: *The score appears at the top-right corner of the screen.*

### Updating the Score as Aliens Are Shot Down

为了在屏幕上写出实时分数，我们在外星人被击中时更新 `stats.score` 的值，然后调用 `prep_score()` 来更新分数图像。但首先，让我们确定玩家每次击落一个外星人时获得多少分：

**settings.py**

```python
def initialize_dynamic_settings(self):
    --snip--

    # Scoring settings
    self.alien_points = 50
```

```python
def _check_play_button(self, mouse_pos):
    --snip--
    if button_clicked and not self.game_active:
        --snip--
        # Reset the game statistics.
        self.stats.reset_stats()
        self.sb.prep_score()
    --snip--
```

我们在开始新游戏时重置游戏统计数据后调用 prep_score()。这会用 0 分为记分板做准备。

### Making Sure to Score All Hits

按当前写法，我们的代码可能会错过对某些外星人的计分。例如，如果在一次循环中两颗子弹与外星人相撞，或者如果我们制作了一颗更宽的子弹以击中多个外星人，玩家只会为击中其中一个外星人获得分数。为了解决这个问题，让我们改进子弹-外星人碰撞的检测方式。

**alien_invasion.py**

```python
def _check_bullet_alien_collisions(self):
    --snip--
    if collisions:
        for aliens in collisions.values():
            self.stats.score += self.settings.alien_points * len(aliens)
        self.sb.prep_score()
    --snip--
```

如果 collisions 字典已被定义，我们会遍历字典中的所有值。记住，每个值都是被一颗子弹击中的外星人列表。我们将每个外星人的值乘以每个列表中的外星人数量，并将此数量加到当前分数上。为了测试这一点，将子弹的宽度改为 300 像素并验证你是否能为用超宽子弹击中的每个外星人获得分数；然后将子弹宽度恢复到正常值。

### Increasing Point Values

由于游戏在玩家达到新等级时变得更困难，后期等级的外星人应该值更多的分数。为了实现此功能，我们将在游戏速度增加时添加增加分数值的代码：

**settings.py**

```python
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        --snip--
        # How quickly the game speeds up
        self.speedup_scale = 1.1
        # How quickly the alien point values increase
        self.score_scale = 1.5

        self.initialize_dynamic_settings()

    def initialize_dynamic_settings(self):
        --snip--

    def increase_speed(self):
        """Increase speed settings and alien point values."""
        self.ship_speed *= self.speedup_scale
        self.bullet_speed *= self.speedup_scale
        self.alien_speed *= self.speedup_scale

        self.alien_points = int(self.alien_points * self.score_scale)
```

