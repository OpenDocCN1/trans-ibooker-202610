
The displayTowers() function calls displayDisk() to display each disk at a specific width, or if 0 is passed, the pole with no disk:

```
# Display the tower labels A, B, and C:
emptySpace = ' ' * (TOTAL_DISKS)
print('{0} A{0}{0} B{0}{0} C\n'.format(emptySpace))
```

We display the A, B, and C labels onscreen. The player needs this information to distinguish between the towers and to reinforce that the towers are labeled A, B, and C rather than 1, 2, and 3 or Left, Middle, and Right. I chose not to use 1, 2, and 3 for the tower labels to prevent players from confusing these numbers with the numbers used for the disks' sizes.

We set the emptySpace variable to the number of spaces to place in between each label, which in turn is based on TOTAL_DISKS, because the more disks in the game, the wider apart the poles are. Rather than use an f-string, as in print(f'{emptySpace} A{emptySpace}{emptySpace} B{emptySpace}{emptySpace} C\n'), we use the format() string method. This allows us to use the same emptySpace argument wherever {0} appears in the associated string, producing shorter and more readable code than the f-string version.

The displayDisk() function displays a single disk along with its width. If no disk is present, it displays just the pole:

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

We represent a disk using a leading empty space, a number of @ characters equal to the disk width, two characters for the width (including an underscore if the width is a single digit), another series of @ characters, and then the trailing empty space. To display just the empty pole, all we need are the leading empty space, two pipe characters, and trailing empty space. As a result, we'll need six calls to displayDisk() with six different arguments for width to display the following tower:

```
||
@ 1@
@@_2@@
@@@_3@@@
@@@@_4@@@@
@@@@@_5@@@@@
```

Notice how the displayTowers() and displayDisk() functions split the responsibility of displaying the towers. Although displayTowers() decides
how to interpret the data structures that represent each tower, it relies on displayDisk() to actually display each disk of the tower. Breaking your program into smaller functions like this makes each part easier to test. If the program displays the disks incorrectly, the problem is likely in displayDisk(). If the disks appear in the wrong order, the problem is likely in displayTowers(). Either way, the section of code you'll have to debug will be much smaller.

To call the main() function, we use a common Python idiom:

```
# If this program was run (instead of imported), run the game:
if __name__ == '__main__':
    main()
```

Python automatically sets the __name__ variable to '__main__' if a player runs the towerofhanoi.py program directly. But if someone imports the program as a module using import towerofhanoi, then __name__ would be set to 'towerofhanoi'. The if __name__ == '__main__': line will call the main() function if someone runs our program, starting a game of Tower of Hanoi. But if we simply want to import the program as a module so we could, say, call the individual functions in it for unit testing, this condition will be False and main() won't be called.

## Four-in-a-Row

Four-in-a-Row is a two-player, tile-dropping game. Each player tries to create a row of four of their tiles, whether horizontally, vertically, or diagonally. It's similar to the board games Connect Four and Four Up. The game uses a 7 by 6 stand-up board, and tiles drop to the lowest unoccupied space in a column. In our Four-in-a-Row game, two human players, X and O, will play against each other, as opposed to one human player against the computer.

### The Output

When you run the Four-in-a-Row program in this chapter, the output will look like this:

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

Try to figure out the many subtle strategies you can use to get four tiles in a row while blocking your opponent from doing the same.

### The Source Code

Open a new file in your editor or IDE, enter the following code, and save it as fourinarow.py:

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
```
