
The decision in favor of the `XBoard` protocol was made in early 2011. The structure of `DokChess` allows us to add alternative communication protocols (`UCI` or other) without having to change the engine itself. See dependencies in the building block view (refer V 5.1, *构建块视图，级别 1*).

The preferred frontend for Windows is `Arena`. It is freely available and excels the functionality of `WinBoard`. It has good debugging facilities. For example, it can present the communication between the frontend and the engine live in a window. `Arena` supports both protocols.

By opting for the `XBoard` protocol, other operating systems (in addition to Windows, especially Mac OS X and Linux) are also supported with freely available frontends (see the preceding table). As such, a larger circle of interested developers may use the engine, which was finally essential.

## 9.2 位置对象是否可变？

### 问题背景

For various `DokChess` modules, game situations on the chessboard (so-called positions) must be provided and exchanged between them. Do we make the associated data structure changeable or unchangeable (immutable)?

During a game, the position changes when the opposing players move their pieces. In addition, the engine performs possible moves in its analysis, tries to counter the moves of the opponent, evaluates the outcoming positions, and then discards moves. The result is a tree that contains many thousands of different positions, depending on the search depth.

Depending on whether the position is immutable as a data structure or not, algorithms are easier or more difficult to implement, and its execution is efficient in a different way.

All modules depend on the `Position` interface. A subsequent change would affect all of `DokChess`.

### 决策的影响因素

#### 约束
* 在 Java 中实现 ( refer V 2.1 *技术约束*)
* 适中的硬件设备 (refer V 2.1 *技术约束*)

#### 显著受影响的质量属性
* 对实验具有吸引力的平台（可变性，refer to V 1.2 *质量目标*)
* 可接受的对弈强度（吸引力，refer to V 1.2 *质量目标*)
* 对对手走法的快速响应（效率，refer to V 1.2 *质量目标*）

#### 受影响的风险
* 实施工作量（see V 11.2 *风险*）
* 达到对弈强度（see V 11.3 *风险*）

#### 假设
* 可以实现一个具有细粒度对象模型（即 `Piece` 和 `Move` 等类）的数据结构，其效率足以在适当的响应时间内提供所需的对弈强度。
* 未来，还应使用该数据结构实现并发算法。

### 考虑的替代方案

The starting point is domain-driven classes for `square`, `piece`, and `move` (refer V 8.2, *国际象棋领域模型*). These classes are realized immutable as value objects (the `e4` field always remains `e4` after its construction).

For the `Position`, two alternatives are considered:
* **选项（1）：位置可变**：接口的各个方法，例如执行走法或撤回走法，会改变对象状态。

*选项 1 的伪代码如下：*
```
Position p = new Position(); // starting position, white side to move
p.performMove(e2e4); // king's pawn advances two squares, black to move
p.takeBackLastMove(); // starting position again
```
* **选项（2）：位置不可变（不可变）**：这意味着执行走法的方法会提供新的位置（它复制旧位置，然后进行走法）作为另一个不可变对象作为结果。

*选项 2 的伪代码如下：*
```
Position p = new Position();
newPosition = p.performMove(e2e4); // p keeps unchanged
```

The following table summarizes the strengths and weaknesses of the two options:

![图 5.41：优势与劣势](image/B15546_05_41.jpg)
###### 图 5.41：优势与劣势

### 选项（1）：位置可变

*(+) 积极论点*

The position with its extensive state is not copied in each move. This saves memory and CPU time, and it treats the garbage collector with care. For analysis algorithms, however, it is necessary to implement functionality that takes back the effect of moves (undo). This `Undo` also takes time, hence the neutral rating for the time behavior.

*(-) 消极论点*

The implementation of an `Undo` functionality is complex. Not only does it have to set up captured pieces back on the board, the castling rule and en passant require special treatment as well. The `Gamma+94` command pattern suggests itself as an option. The use of that pattern within algorithms is also more complex because the latter must explicitly call methods to take back the moves.

Finally, changeable state has drawbacks related to concurrency.

### 选项（2）：位置不可变

*(+) 积极论点*

When you perform a move, the position is copied. It does not change the original position. This eliminates the implementation of move reversal (`Undo`). Clients can simply store the old position as a value. This saves effort in the implementation compared to option (1).

Immutable objects offer significant advantages in concurrent algorithms.

*(-) 消极论点*

Copying the state for each new position takes time. When analyzing situations, it covers many positions, which can take a lot of time.

Beyond that, copying the state for each new position costs memory. The implementation of search algorithms with backtracking at least prevents complete game trees from ending up on the heap. Nevertheless, more memory is required, and the garbage collector has more work to do.

Both points have a negative effect on efficiency.

### 决策

The decision for the unchangeable position (option 2) was made in early 2011 due to the advantages in terms of ease of implementation and the prospect of exploiting concurrency. All the disadvantages of option 2 are related to efficiency.

Due to the risk of failing to achieve the objectives with respect to the playing strength in an acceptable computing time (attractiveness, efficiency), prototypes of both versions have been implemented and compared in a mate search (checkmate in three moves) with the `minimax` algorithm. With option 2, the search took 30% longer, assuming you implement copying efficiently. But this option met the constraints as well.

Further optimization options could reduce the efficiency disadvantage compared with option 1, if necessary. But they have not been implemented in the first run in order to keep the implementation simple.

These options included the use of multiple processors/cores with concurrent algorithms. In the meantime (with `DokChess` 2.0), this was illustrated with a parallel `minimax` example.

## V.10 质量场景

The quality scenarios in this section depict the fundamental quality goals of `DokChess` as well as other required quality properties. They allow the evaluation of decision alternatives.

### 10.1 效用树

The following diagram gives an overview of the relevant quality attributes and their associated scenarios:

![图 5.42：效用树](image/B15546_05_42.jpg)
###### 图 5.42：效用树

### 10.2 评估场景

The evaluation scenarios can be listed as follows:
1. A person with basic knowledge of UML and chess looks for an introduction to the `DokChess` architecture. They get the idea of the solution strategy and the essential design within 15 minutes.
2. An architect wishes to apply `arc42` searches to a concrete example for an arbitrary section of the template. They find the relevant content immediately in the documentation.
3. An experienced Java developer searches for the implementation of a module described in the design. They find it in the source code without detours or help from others.
