
All DokChess subsystem methods throw runtime exceptions. In addition, *引擎子系统错误消息* (`onError`) may occur during an asynchronous search. Your own extensions (for example, a custom search) must be implemented accordingly. Checked exceptions (for example, `java.io.IOException`) need to be wrapped in runtime exceptions.

The Javadoc of methods and constructors in question shows the rare cases where exceptions are expected in DokChess, such as difficulty with reading or opening a book file, or when trying move calculations within the engine at an invalid position (if identified). All other exceptions would be programming errors (please report them at [https://github.com/DokChess/](https://github.com/DokChess/)).

The XBoard subsystem catches all exceptions and communicates them via the XBoard protocol outwards (using the `tellusererror` command). A graphical frontend usually visualizes them in an error dialog or alert box. The following screenshot depicts that for the Arena chess frontend:

![图 5.37：Arena 中的错误对话框](image/B15546_05_37.jpg)
*图 5.37：Arena 中的错误对话框*

DokChess continues to work normally. The client decides whether proceeding in this specific situation makes sense. For example, it could continue to play without an opening book.

## 8.6 日志记录与追踪
For improvements and extensions, the analysis capabilities provided by DokChess are of note, particularly in case of an error.

The DokChess functionality can be checked easily with unit tests. This is particularly true for the correct implementation of the game rules and for the gameplay of the engine (refer V 8.7, *可测试性*). It holds true for your own extensions in this area as well.

Therefore, there is no fine-grained logging output within DokChess. Solutions such as SLF4J ([https://www.slf4j.org](https://www.slf4j.org)) are not present. In this way, we avoid dependency on an external library, and the source code is not polluted by this aspect at all.

For communication between the client and DokChess via the XBoard protocol, in addition to interactive operations via a shell (refer, Concept *V 8.3 用户界面*), the client may monitor the conversation. Common chess frontends permit this by writing log files and/or a simultaneous display of a log window during the game. The following screenshot shows this functionality within Arena:

![图 5.38：Arena 中 XBoard 协议的日志和调试窗口](image/B15546_05_38.jpg)
*图 5.38：Arena 中 XBoard 协议的日志和调试窗口*

If the engine blocks and it is unclear what went on the XBoard protocol, such tools are simply invaluable. Due to the availability of this feature, communication protocol tracing was not implemented within DokChess at all.

## 8.7 可测试性
Nothing is more embarrassing for an engine than an illegal move.

The functionality of the individual modules of DokChess is ensured by extensive unit tests. You find a `src/test` folder next to `src/main`, where the Java source code of the modules is stored. It mirrors the package structure, and in the corresponding packages, unit tests for the classes are implemented with JUnit 4 ([https://junit.org/junit4/](https://junit.org/junit4/)).

Standard unit testing, which examines individual classes, is named after the class itself with the suffix `Test`. In addition, there are tests that examine the interaction of modules, and in extreme cases, the whole system. With the help of such tests, the correct playing of DokChess is ensured. More complex, long-running integration tests are in `src/integTest`. This includes playing entire games.

In many tests, positions must be provided as call parameters. Here, the **Forsyth-Edwards 记法** (**FEN**) is used. This notation allows the specification of a complete game scenario as a compact string without a line break and is therefore perfect for use in automated tests.

The starting position in FEN is as follows:

```
rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1
```

Lowercase letters stand for black pieces, and uppercase letters stand for white pieces. For the piece types, the initials of the English names (r for rook, p for pawn, and so on) are used:

![图 5.39：示例局面](image/B15546_05_39.jpg)
*图 5.39：示例局面*

The game situation in the preceding diagram, with white before its 79th move, with 30 half-moves long no piece was captured and no pawn was moved, looks like this in FEN:

```
6r1/6pp/7r/1B5K/1P3k2/N7/3R4/8 w - - 30 79
```

And it reads 6 squares free, black rook, square free, new rank ….

Details about the notation can be found on Wikipedia at [https://en.wikipedia.org/wiki/Forsyth–Edwards_Notation](https://en.wikipedia.org/wiki/Forsyth-Edwards_Notation). The `Position` class has a constructor that accepts a string in FEN. The `toString()` method of this class also provides FEN.

## V.9 设计决策
This section enables you to understand two fundamental design decisions of DokChess in detail.

### 9.1 引擎如何与外部世界通信？
#### 问题背景
As a central requirement, DokChess must work together with existing chess frontends. How do we connect them?

A whole series of graphical user interfaces are available for playing against chess programs. Moreover, there are software solutions with a larger scope for chess enthusiasts. In addition to the game *Human vs. Machine*, they offer more functionality, such as analyzing games. Over time, new chess programs will be released – and others will possibly disappear from the market.

Depending on how the connection with such programs is realized, DokChess can or cannot communicate with specific frontend clients. Thus, the issue affects DokChess' interoperability with existing chess software and its adaptability to future chess software.

#### 决策影响因素
#### 约束条件
The following are the constraints that influence the decisions for designing:
* Operation of the frontend at least on Windows desktop operating systems
* Support for freely available frontends
* Favoring established (chess) standards (see V 2.3 *惯例*)

#### 显著受影响的质量属性
* Using existing frontends (see V 1.2 *互操作性*, *质量目标*)
* Platform appealing to experiments (see V 1.2 *可变性*, *质量目标*)
* Adaptability to future chess software

#### 受影响的风险
Connecting to a frontend is a considerable risk (see V 11.1 *风险*).

#### 假设
The investigation of just a few available frontends leads to all interesting integration options.

#### 考虑的替代方案
In early 2011, the following chess frontends were investigated:
* Arena Chess GUI (available for free, runs on Windows)
* Fritz for Fun (commercially provided by ChessBase GmbH, runs on Windows)
* WinBoard/XBoard (Open source, runs on Windows, Mac OS X, *nix)

As a result, two communication protocols have been identified as options:
* **选项 1**: UCI Protocol (**通用棋类接口**), see *MeyerKahlen2004*
* **选项 2**: XBoard protocol (also known as Winboard and as Chess Engine Communication Protocol), see *Mann+2009*

Neither of the two protocols are formally specified, but both are publicly documented. Both protocols are text-based, and communication between the frontend and the engine is via stdin/stdout. In both cases, the frontend starts the engine in a separate process.

The following table compares the three investigated frontends to their implemented protocols:

![图 5.40：前端比较](image/B15546_05_40.jpg)
*图 5.40：前端比较*

#### 决策
Under the given constraints, the quality goals can generally be achieved with both options. Depending on which protocol is implemented, different frontends are supported.
