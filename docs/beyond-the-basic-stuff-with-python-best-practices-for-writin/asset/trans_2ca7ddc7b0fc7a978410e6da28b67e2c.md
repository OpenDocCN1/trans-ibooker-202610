
displayTowers() 函数调用 displayDisk() 来以特定宽度显示每个圆盘，或者如果传入 0，则显示没有圆盘的杆：

```
# Display the tower labels A, B, and C:
emptySpace = ' ' * (TOTAL_DISKS)
print('{0} A{0}{0} B{0}{0} C\n'.format(emptySpace))
```

我们在屏幕上显示 A、B 和 C 标签。玩家需要这些信息来区分塔，并强化塔的标签是 A、B 和 C，而不是 1、2 和 3 或 左、中、右。我选择不使用 1、2 和 3 作为塔的标签，以防止玩家将这些数字与用于圆盘大小的数字混淆。

我们将 emptySpace 变量设置为放置在每个标签之间的空格数，这反过来又基于 TOTAL_DISKS，因为游戏中圆盘越多，杆子之间的距离就越宽。与其使用 f-string，如 print(f'{emptySpace} A{emptySpace}{emptySpace} B{emptySpace}{emptySpace} C\n')，我们使用 format() 字符串方法。这使我们能够在相关字符串中出现 {0} 的任何位置使用相同的 emptySpace 参数，从而产生比 f-string 版本更短、更易读的代码。

displayDisk() 函数显示单个圆盘及其宽度。如果没有圆盘，则只显示杆：

```
def displayDisk(width):
    """Display a disk of the given width. A width of 0 means no disk."""
    emptySpace = ' ' * (TOTAL_DISKS - width)
    if width == 0:
        # Display a pole segment without a disk:
        print(f'{emptySpace}||{emptySpace}', end='')
    else:
        # Display the disk:
        disk = '@' * width
        numLabel = str(width).rjust(2, '_')
        print(f"{emptySpace}{disk}{numLabel}{disk}{emptySpace}", end='')
```

我们使用前导空格、等于圆盘宽度的 @ 字符数、宽度对应的两个字符（如果宽度为单位数则包括下划线）、另一系列 @ 字符，然后是尾随空格来表示一个圆盘。要仅显示空杆，我们只需要前导空格、两个竖线字符和尾随空格。因此，我们需要六次调用 displayDisk()，使用六个不同的宽度参数来显示以下塔：

```
||
@ 1@
@@_2@@
@@@_3@@@
@@@@_4@@@@
@@@@@_5@@@@@
```

注意 displayTowers() 和 displayDisk() 函数如何分担显示塔的职责。尽管 displayTowers() 决定如何解释表示每个塔的数据结构，但它依赖于 displayDisk() 来实际显示塔的每个圆盘。像这样将程序分解为更小的函数使每个部分更容易测试。如果程序错误地显示圆盘，问题很可能出在 displayDisk() 中。如果圆盘出现顺序错误，问题很可能出在 displayTowers() 中。无论哪种情况，你需要调试的代码部分都会小得多。

要调用 main() 函数，我们使用一种常见的 Python 惯用法：

```
# If this program was run (instead of imported), run the game:
if __name__ == '__main__':
    main()
```

如果玩家直接运行 towerofhanoi.py 程序，Python 会自动将 __name__ 变量设置为 '__main__'。但如果有人使用 import towerofhanoi 将程序作为模块导入，那么 __name__ 将被设置为 'towerofhanoi'。if __name__ == '__main__': 行会在有人运行我们的程序时调用 main() 函数，开始一局汉诺塔游戏。但如果我们只是想将程序作为模块导入，以便例如在单元测试中调用其中的各个函数，则此条件将为 False，且 main() 不会被调用。

## Four-in-a-Row

四连珠是一款双人落子游戏。每个玩家都试图创建四个自己棋子的连线，无论是水平、垂直还是对角线。它类似于桌游 Connect Four 和 Four Up。游戏使用 7×6 的直立棋盘，棋子会落到列中最低的未占用空间。在我们的四连珠游戏中，两名人类玩家 X 和 O 将相互对战，而不是一名人类玩家对抗电脑。

### The Output

当你在本章中运行四连珠程序时，输出将如下所示：

```
Four-in-a-Row, by Al Sweigart al@inventwithpython.com

Two players take turns dropping tiles into one of seven columns, trying
to make four in a row horizontally, vertically, or diagonally.

    1234567
    +-------+
    |.......|
    |.......|
    |.......|
    |.......|
    |.......|
    +-------+
```

```
Player X, enter 1 to 7 or QUIT:
> 1

    1234567
    +-------+
    |.......|
    |.......|
    |.......|
    |.......|
    |.......|
    |X......|
    +-------+
Player O, enter 1 to 7 or QUIT:
--snip--
Player O, enter 1 to 7 or QUIT:
> 4

    1234567
    +-------+
    |.......|
    |.......|
    |...O...|
    |X.OO...|
    |X.XO...|
    |XOXO.X.|
    +-------+
Player O has won!
```

尝试找出在阻止对手同时完成的同时获得四连珠的许多微妙策略。

### The Source Code

在编辑器或 IDE 中打开一个新文件，输入以下代码，并将其保存为 fourinarow.py：

```
"""Four-in-a-Row, by Al Sweigart al@inventwithpython.com
A tile-dropping game to get four-in-a-row, similar to Connect Four."""

import sys

# Constants used for displaying the board:
EMPTY_SPACE = "."  # A period is easier to count than a space.
PLAYER_X = "X"
PLAYER_O = "O"

# Note: Update BOARD_TEMPLATE & COLUMN_LABELS if BOARD_WIDTH is changed.
BOARD_WIDTH = 7
BOARD_HEIGHT = 6
COLUMN_LABELS = ("1", "2", "3", "4", "5", "6", "7")
assert len(COLUMN_LABELS) == BOARD_WIDTH

# The template string for displaying the board:
BOARD_TEMPLATE = """
1234567
+-------+
|{}{}{}{}{}{}{}|
|{}{}{}{}{}{}{}|
|{}{}{}{}{}{}{}|
|{}{}{}{}{}{}{}|
|{}{}{}{}{}{}{}|
|{}{}{}{}{}{}{}|
+-------+"""

def main():
    """Runs a single game of Four-in-a-Row."""
    print(
        """Four-in-a-Row, by Al Sweigart al@inventwithpython.com

Two players take turns dropping tiles into one of seven columns, trying
to make Four-in-a-Row horizontally, vertically, or diagonally.
"""
    )

    # Set up a new game:
    gameBoard = getNewBoard()
    playerTurn = PLAYER_X

    while True:  # Run a player's turn.
        # Display the board and get player's move:
        displayBoard(gameBoard)
        playerMove = getPlayerMove(playerTurn, gameBoard)
        gameBoard[playerMove] = playerTurn

        # Check for a win or tie:
        if isWinner(playerTurn, gameBoard):
            displayBoard(gameBoard)  # Display the board one last time.
            print("Player {} has won!".format(playerTurn))
            sys.exit()
        elif isFull(gameBoard):
            displayBoard(gameBoard)  # Display the board one last time.
            print("There is a tie!")
            sys.exit()

        # Switch turns to other player:
        if playerTurn == PLAYER_X:
            playerTurn = PLAYER_O
        elif playerTurn == PLAYER_O:
            playerTurn = PLAYER_X

def getNewBoard():
    """Returns a dictionary that represents a Four-in-a-Row board.

    The keys are (columnIndex, rowIndex) tuples of two integers, and the
    values are one of the "X", "O" or "." (empty space) strings."""
    board = {}
    for rowIndex in range(BOARD_HEIGHT):
        for columnIndex in range(BOARD_WIDTH):
            board[(columnIndex, rowIndex)] = EMPTY_SPACE
    return board

def displayBoard(board):
    """Display the board and its tiles on the screen."""

    # Prepare a list to pass to the format() string method for the board
    # template. The list holds all of the board's tiles (and empty
    # spaces) going left to right, top to bottom:
    tileChars = []
    for rowIndex in range(BOARD_HEIGHT):
        for columnIndex in range(BOARD_WIDTH):
            tileChars.append(board[(columnIndex, rowIndex)])

    # Display the board:
    print(BOARD_TEMPLATE.format(*tileChars))

def getPlayerMove(playerTile, board):
    """Let a player select a column on the board to drop a tile into.

    Returns a tuple of the (column, row) that the tile falls into."""
    while True:  # Keep asking player until they enter a valid move.
        print(f"Player {playerTile}, enter 1 to {BOARD_WIDTH} or QUIT:")
        response = input("> ").upper().strip()

        if response == "QUIT":
            print("Thanks for playing!")
            sys.exit()

        if response not in COLUMN_LABELS:
            print(f"Enter a number from 1 to {BOARD_WIDTH}.")
            continue  # Ask player again for their move.

        columnIndex = int(response) - 1  # -1 for 0-based column indexes.

        # If the column is full, ask for a move again:
        if board[(columnIndex, 0)] != EMPTY_SPACE:
            print("That column is full, select another one.")
            continue  # Ask player again for their move.

        # Starting from the bottom, find the first empty space.
        for rowIndex in range(BOARD_HEIGHT - 1, -1, -1):
            if board[(columnIndex, rowIndex)] == EMPTY_SPACE:
                return (columnIndex, rowIndex)

def isFull(board):
    """Returns True if the `board` has no empty spaces, otherwise
    returns False."""
    for rowIndex in range(BOARD_HEIGHT):
        for columnIndex in range(BOARD_WIDTH):
