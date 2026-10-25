

## 11.4 一个随机整数游戏

用 Python 编写的最简单的游戏之一是猜数字游戏，用户反复猜测程序预先选定的一个秘密数字。在每一轮中，用户做出一个猜测，程序会通过说“成功”（用户获胜）、“太高”或“太低”来响应。这个游戏的一个简单版本在第 1 章中介绍过。

除非程序选择的秘密数字每次都不同，否则游戏就无趣；此外，这个数字应该尽可能不可预测，这就是随机数的全部意义所在。

下面是游戏简单版本的代码。这个版本首先从 1 到 50（包含 50）中选取一个随机数。

```python
import random
n = random.randint(1, 50)
while True:
    guess = int(input('Enter guess: '))
    if guess == n:
        print('Success! You win.')
        break
    elif guess < n:
        print('Too low.', end=' ')
    else:
        print('Too high.', end=' ')
```

下面是一个示例会话。假设函数调用 `random.randint(1, 50)` 返回值 31。用户直到游戏结束才知道这个值已被选中。

```
Enter guess: 25
Too low. Enter guess: 37
Too high. Enter guess: 30
Too low. Enter guess: 34
Too high. Enter guess: 32
Too high. Enter guess: 31
Success! You win.
```

这个游戏可以改进几个方面。首先，每局游戏结束后应该询问用户是否想再玩一次。其次，如果用户在某一轮游戏中感到无聊，应该能够提前退出。下面是改进后的版本。

```python
import random
def play_the_game():
    n = random.randint(1, 50)
    while True:
        guess = int(input('Enter guess (0 to exit): '))
        if guess == 0:
            print('Exiting game.')
            break
        elif guess == n:
            print('Success! You win.')
            break
        elif guess < n:
            print('Too low.', end=' ')
        else:
            print('Too high.', end=' ')

while True:
    play_the_game()
    ans = input('Want to play again? (Y or N): ')
    if not ans or ans[0] in 'Nn':
        break
```

## 11.5 创建 Deck 对象

**shuffle** 函数是 **random** 包中最有用的函数之一。正如你可能猜到的，这个函数特别适用于模拟一副扑克牌，你猜对了。但它也可以扩展到其他情况。

**shuffle** 的作用是重新排列列表的顺序，使任何元素都可以出现在任何位置。元素的数量不会改变，重复项的数量（如果有）也不会改变。因此，例如，假设你打乱下面的列表：

```python
kings_list = ['John', 'James', 'Henry', 'Henry',
              'George']
```

接下来，我们使用 `random.shuffle` 来随机化顺序。

```python
random.shuffle(kings_list)
```

如果你现在打印列表，你会发现无论打乱如何，仍然有两个 Henry，以及各一个 John、James 和 George。不过，顺序几乎肯定会改变。

打乱算法是一个相当通用的算法。

```
For I in range(0, N-2),
J = randint(I, N-1)
Swap list[I] with list[J]
```

`random.shuffle` 函数的作用是原地重新排列列表，替换所有旧值，但不会在内存中移动列表本身。

封装一副牌功能的最佳方法之一是创建一个 Deck 类，并用它来实例化 Deck 对象。它应该具有以下属性。

- 它将包含 0 到 N 的数字列表。对于一副 52 张的牌，这些数字中的每一个都可以映射到一张具有唯一等级和花色的牌。
- 初始化时，Deck 对象会自行打乱。
- 然后你可以要求 Deck 对象从“顶部”（列表的开头）一次发一张牌，将牌作为 0 到 51 之间的数字返回。
- 当所有牌都发完后，Deck 对象会自动重新打乱。

完成后，这将是面向对象编程的一个绝佳示例。Deck 对象的实例将维护其内部状态。以下实现使你能够创建任何大小的自动重新打乱的牌组。

```python
import random

class Deck():
    def __init__(self, size):
        self.card_list = [i for i in range(size)]
        random.shuffle(self.card_list)
        self.current_card = 0
        self.size = size

    def deal(self):
        if self.size - self.current_card < 1:
            random.shuffle(self.card_list)
            self.current_card = 0
            print('Reshuffling...!!!!')
        self.current_card += 1
        return self.card_list[self.current_card - 1]
```

顺便说一句，牌组“发出”的值可以转换为具有唯一等级和花色组合的扑克牌。

```python
ranks = ['2', '3', '4', '5', '6', '7', '8', '9',
         '10', 'J', 'Q', 'K', 'A']
suits = ['clubs', 'diamonds', 'hearts', 'spades']
my_deck = Deck(52)

# Deal twelve poker hands, so user can compare
# before and after shuffling.

for i in range(12):
    for i in range(5):
        d = my_deck.deal()
        r = d % 13
        s = d // 13
        print(ranks[r], 'of', suits[s])
    print()
```

正在进行的牌会留在桌面上，从发牌时开始。只有在发新手牌时，正在进行的牌才会加入弃牌堆。这在牌组、正在进行的牌和弃牌堆之间建立了一种关系，如图 11.1 所示。

![Figure 11.1. Movement of cards within a Deck object](img/6e45221a8147f2442c653a3d4a866570_671_0.png)

曾经，二十一点（也称为黑杰克）在赌场中就是这样玩的。偶尔现在仍然是这样：一副标准牌组，一直发到最后一张牌，然后重新打乱。

我们可以重写 Deck 对象如下。

```python
import random

class Deck():
    def __init__(self, size):
        self.card_list = [i for i in range(size)]
        self.cards_in_play_list = []
        self.discards_list = []
        random.shuffle(self.card_list)

    def deal(self):
        if len(self.card_list) < 1:
            random.shuffle(self.discards_list)
            self.card_list = self.discards_list
            self.discards_list = []
            print('Reshuffling...!!!!')
        new_card = self.card_list.pop()
        self.cards_in_play_list.append(new_card)
        return new_card

    def new_hand(self):
        self.discards_list += self.cards_in_play_list
        self.cards_in_play_list.clear()
```

这个类定义有一个新方法 `new_hand`，每当一手牌结束并且当前正在进行的所有牌都放入弃牌堆时，都应该调用它。然后牌组应该将当前正在进行的牌添加到 `discard_list` 并清除 `cards_in_play_list`。

对 `deal` 方法的更改更为复杂。现在，与其只是打乱通常包含牌组中所有牌的 `card_list`，不如只打乱弃牌堆。然后将结果列表与 `card_list` 互换；这成为新的抽牌牌组。然后清空弃牌堆。

如果在桌上仍有牌时发生重新打乱，那些牌不会被重新打乱，因此生成的牌组大小可能不同。但是那些正在进行的牌如何才能重新进入牌组呢？很简单。它们将在当前手牌结束时加入弃牌堆，然后最终被重新打乱回牌组。

> **Note**  
> 你可能希望根据拉斯维加斯赌场二十一点规则的变化对这个类进行进一步修改。例如，你可能希望容纳大多数赌场使用的六副牌“鞋”。这实际上只是分配合适的牌组大小的问题；它不会改变此处显示的代码。你还可能希望修改一些方法，以便发牌员有方法提前重新打乱（例如，编写一个新方法来仅执行此操作）。

## 11.6 为牌组添加图形符号

如果你愿意，可以更改 Deck 类的初始化，使其存储标准扑克牌的小图片，而不仅仅是存储数字。如果你这样做，那么你就不需要一段单独的代码来将 0 到 51 的数字翻译成扑克牌的名称。相反，你可以直接打印牌符号，如下面的版本所示。

```python
def __init__(self, n_decks=1):
    self.card_list = [num + suit
                      for suit in '\u2665\u2666\u2663\u2660'
                      for num in 'A23456789TJQK'
                      for deck in range(n_decks)]
```

