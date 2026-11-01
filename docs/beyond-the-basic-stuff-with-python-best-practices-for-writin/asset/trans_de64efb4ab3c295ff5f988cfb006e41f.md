
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，这个程序与非面向对象的井字游戏程序相同。输出看起来完全一样。我们将原本在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法中，因为它们执行相同的任务，即准备棋盘数据结构。我们将其他函数转换为方法，用 self 参数替换了旧的 board 参数，因为它们也具有类似的目的：它们都是操作井字游戏棋盘数据结构的代码块。

当这些方法中的代码需要更改存储在 _spaces 属性中的字典时，代码使用 self._spaces。当这些方法中的代码需要调用其他方法时，调用也会以 self 和一个点号为前缀。这与“Creating a Simple Class: WizCoin”中 coinJars.values() 的情况类似，其中 coinJars 变量中有一个对象。在此示例中，具有要调用的方法的对象位于 self 变量中。

另外，请注意 _spaces 属性以下划线开头，这意味着只有 TTTBoard 的方法内部的代码才应访问或修改它。类外部的代码只能通过调用修改它的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，这个程序与非面向对象的井字游戏程序相同。输出看起来完全一样。我们将原本在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法中，因为它们执行相同的任务，即准备棋盘数据结构。我们将其他函数转换为方法，用 self 参数替换了旧的 board 参数，因为它们也具有类似的目的：它们都是操作井字游戏棋盘数据结构的代码块。

当这些方法中的代码需要更改存储在 _spaces 属性中的字典时，代码使用 self._spaces。当这些方法中的代码需要调用其他方法时，调用也会以 self 和一个点号为前缀。这与“Creating a Simple Class: WizCoin”中 coinJars.values() 的情况类似，其中 coinJars 变量中有一个对象。在此示例中，具有要调用的方法的对象位于 self 变量中。

另外，请注意 _spaces 属性以下划线开头，这意味着只有 TTTBoard 的方法内部的代码才应访问或修改它。类外部的代码只能通过调用修改它的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，这个程序与非面向对象的井字游戏程序相同。输出看起来完全一样。我们将原本在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法中，因为它们执行相同的任务，即准备棋盘数据结构。我们将其他函数转换为方法，用 self 参数替换了旧的 board 参数，因为它们也具有类似的目的：它们都是操作井字游戏棋盘数据结构的代码块。

当这些方法中的代码需要更改存储在 _spaces 属性中的字典时，代码使用 self._spaces。当这些方法中的代码需要调用其他方法时，调用也会以 self 和一个点号为前缀。这与“Creating a Simple Class: WizCoin”中 coinJars.values() 的情况类似，其中 coinJars 变量中有一个对象。在此示例中，具有要调用的方法的对象位于 self 变量中。

另外，请注意 _spaces 属性以下划线开头，这意味着只有 TTTBoard 的方法内部的代码才应访问或修改它。类外部的代码只能通过调用修改它的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，这个程序与非面向对象的井字游戏程序相同。输出看起来完全一样。我们将原本在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法中，因为它们执行相同的任务，即准备棋盘数据结构。我们将其他函数转换为方法，用 self 参数替换了旧的 board 参数，因为它们也具有类似的目的：它们都是操作井字游戏棋盘数据结构的代码块。

当这些方法中的代码需要更改存储在 _spaces 属性中的字典时，代码使用 self._spaces。当这些方法中的代码需要调用其他方法时，调用也会以 self 和一个点号为前缀。这与“Creating a Simple Class: WizCoin”中 coinJars.values() 的情况类似，其中 coinJars 变量中有一个对象。在此示例中，具有要调用的方法的对象位于 self 变量中。

另外，请注意 _spaces 属性以下划线开头，这意味着只有 TTTBoard 的方法内部的代码才应访问或修改它。类外部的代码只能通过调用修改它的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，这个程序与非面向对象的井字游戏程序相同。输出看起来完全一样。我们将原本在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法中，因为它们执行相同的任务，即准备棋盘数据结构。我们将其他函数转换为方法，用 self 参数替换了旧的 board 参数，因为它们也具有类似的目的：它们都是操作井字游戏棋盘数据结构的代码块。

当这些方法中的代码需要更改存储在 _spaces 属性中的字典时，代码使用 self._spaces。当这些方法中的代码需要调用其他方法时，调用也会以 self 和一个点号为前缀。这与“Creating a Simple Class: WizCoin”中 coinJars.values() 的情况类似，其中 coinJars 变量中有一个对象。在此示例中，具有要调用的方法的对象位于 self 变量中。

另外，请注意 _spaces 属性以下划线开头，这意味着只有 TTTBoard 的方法内部的代码才应访问或修改它。类外部的代码只能通过调用修改它的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，该程序与非面向对象的井字游戏程序相同，输出也完全一致。我们把原来在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法，因为两者都用于准备棋盘数据结构。我们把其他函数改写为方法，用 self 参数取代原来的 board 参数，因为它们同样是对井字游戏棋盘数据结构进行操作的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，该程序与非面向对象的井字游戏程序相同，输出也完全一致。我们把原来在 getBlankBoard() 中的代码移到了 TTTBoard 类的 __init__() 方法，因为两者都用于准备棋盘数据结构。我们把其他函数改写为方法，用 self 参数取代原来的 board 参数，因为它们同样是对井字游戏棋盘数据结构进行操作的[content]
```
for space in ALL_SPACES:
    if self._spaces[space] == BLANK:
        return False  # If a single space is blank, return False.
    return True  # No spaces are blank, so return True.

def updateBoard(self, space, player):
    """Sets the space on the board to player."""
    self._spaces[space] = player

if __name__ == '__main__':
    main() # Call main() if this module is run, but not when imported.
```

功能上，该程序与非面向对象的井字游戏程序相同。
