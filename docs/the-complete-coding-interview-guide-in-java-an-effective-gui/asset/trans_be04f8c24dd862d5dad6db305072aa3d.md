

## 设计 Google Drive、Google Photos 和 Dropbox（一个全球文件存储和共享服务）

需要解决的问题：

- 如何设计用户功能，如上传、搜索、查看和共享文件/照片？
- 如何跟踪文件共享的权限？
- 如何允许一组用户编辑同一份文档？

## 设计 Twitter、Facebook 和 Instagram（一个极大规模的社交媒体服务）

需要解决的问题：

- 如何高效地存储和搜索帖子/推文？
- 如何实现信息流生成？
- 如何处理社交图谱（参见 编程挑战 13）？

## 设计 Lyft、Uber 和 RideAustin（一个拼车服务）

需要解决的问题：

- 如何将乘车请求与附近的司机匹配？
- 如何存储数百万个骑手和司机不断移动的位置（地理坐标）？
- 如何更新司机/骑手的位置（每秒更新一次）？

## 设计类型提示和网络爬虫（一个与搜索引擎相关的服务）

需要解决的问题：

- 如何刷新数据？
- 如何存储之前的搜索查询？
- 如何检测已输入字符串的最佳匹配？

## 系统可扩展性

- 当用户输入过快时如何处理？
- 如何找到新页面（网页）？
- 如何为动态变化的网页分配优先级？
- 如何保证爬虫不会永远卡在同一个域名上？

## 设计 API 速率限制器（例如 GitHub 或 Firebase）

- 如何在时间窗口内限制请求数量（例如，每秒 30 个请求）？
- 如何在服务器集群中实现速率限制？
- 如何处理节流（软限制和硬限制）？

## 设计附近地点/朋友和 Yelp（一个邻近服务器）

- 如何搜索附近的朋友或地点？
- 如何对地点进行排名？
- 如何根据人口密度存储位置数据？

回答这些挑战并非易事，需要丰富的经验。然而，如果你是一名初级/中级程序员，并且已经阅读了关于可扩展性的本入门章节，那么你应该能够判断你的职业道路是否应该朝这个方向发展。然而，请记住，设计大规模分布式系统是软件工程面试中一个非常苛刻的领域。

## 总结

这是本书的最后一章。我们刚刚涵盖了一系列属于可扩展性主题的问题。

恭喜你走到这一步！现在，在本书的结尾，记住要尽可能多地练习，对自己的判断充满信心，永不放弃！我真的希望你的下一个 Java 职位能带给你梦想中的工作，并且这本书能为你的成功做出贡献。

## 你可能喜欢的其他书籍

如果你喜欢这本书，你可能对 Packt 出版的其他这些书籍感兴趣：

![](img/6709a71136a6b327c831c6c15aff2f6a_765_0.png)

### Java 编程问题

Anghel Leonard

ISBN: 9781789801415

- 在你的应用程序中采用最新的 JDK 11 和 JDK 12 功能
- 解决与集合和数据结构相关的前沿问题
- 使用 lambda 掌握函数式编程风格
- 执行异步通信和并行数据处理
- 使用最新的 Java API 解决字符串和数字问题
- 熟悉 Java 中对象不可变性的不同方面
- 实施正确的做法和干净代码技术

![](img/6709a71136a6b327c831c6c15aff2f6a_766_0.png)

### 学习 Java 12 编程

Nick Samoylov

ISBN: 9781789957051

- 学习并应用面向对象原则
- 深入了解数据结构并理解它们在 Java 中的使用方式
- 探索多线程、异步、函数式和响应式编程
- 为你的应用程序添加用户友好的图形界面
- 了解流是什么以及它们如何帮助数据处理
- 发现微服务的重要性并使用它们使你的应用更健壮和可扩展
- 探索 Java 设计模式和最佳实践以解决日常问题
- 学习编写高质量 Java 代码的技术和惯用法

## 索引

- Abcd application 643, 644
- abstract class
  - about 115
  - versus interfaces 115
  - without abstract method 115
- Abstract Data Type (ADT) 380
- abstraction 67, 69
- Abstraction/AbstractionViaInterface 69
- acyclic graph 481
- adapter patterns
  - versus bridge patterns 122
- adjacency list 483
- adjacency matrix 482
- aggregation 81-83
- algorithms, efficiency
  - best case 154
  - expected case 154
  - worst case 154
- Amazon interview 28
- arrays 290
- ASCII characters 290
- association 79-81
- AVLTreeImpl application 476

- balanced binary tree 474
- base case recursion 590
- best online practices, for gaining experience about 12
- certifications 17
- GitHub account, starting 13
- Learn->Code practice 17
- open source projects, contributing to 13
- rules 12
- Stack Overflow account, starting 14
- technical articles, posting 15
- technical blog, starting 15
- work (portfolio), promoting 15
- YouTube channel, starting 14
- BestTimeToBuySellStock application about 356, 367
- scenarios 356-366
- BiggestColorSpot application 193-196
- Big O cheat sheet reference link 154
- Big O examples
- Big O expressions, reducing 170
- digits 175
- factorial Big O 173

half of array, looping 170
half of matrix, looping 168, 169
in-order traversal, of binary tree 164
log n runtimes 161-163
looping, with O(log n) 171
memoization 166-168
n may vary 165, 166
n notation, using 174
non-dominant terms, dropping 158, 159
number of iteration counts, in Big O 175
O(1) loops, identifying 169, 170
O(n), dropping constants 156-158
O(n), linear time algorithms 155
recursive runtimes 163, 164
sorting 176, 177
string comparison 172
sum, and count 174, 175
binary heap 479-481
binary search algorithm 324
BinarySearch application 580
binary search tree (BST) 473, 502, 616
BinarySearchTreeDuplicates application 526
BinarySearchTreeKthLargestElement application 532
BinarySearchTreeSuccessor application 507
BinarySearchTreeSum application 543, 546
BinarySearchTreeTraversal application 473
binary tree 469, 470
BinaryTreeBalanced application 501
BinaryTreeCommonAncestor application 511
BinaryTreeDistanceFromLeaf application 541
BinaryTreeIsBST application 504
BinaryTreeLandingReservation application 498
BinaryTreeMaxPathSum application 519
BinaryTreePrintCorners application 516
BinaryTreePrintDiagonal application 524
BinaryTreeRightView application 531
BinaryTreeSpiralTraversal application 539
BinaryTreeSubtree application 493
binary tree traversal 470
BinaryTreeTraversal application 472
BinaryTreeVerticalSum application 548
binding operation 116
bin sort algorithm. See also bucket sort algorithm
bit manipulation
  about 236
  binary representation, obtaining of Java integer 236, 237
bit shift operators, using 239
bitwise operators, using 237, 238
coding challenges 242
tips and tricks 241, 242
bit shift operators
  negative integers, in Java 239, 240
  Signed Left Shift [<<] 239
  Signed Right Shift [>>] 240
  Unsigned Right Shift [>>>] 241
  using 239
Bitwise AND [&] 237, 238
Bitwise Exclusive OR (XOR) [^] 238
bitwise operators
  Bitwise AND [&] 237
  Bitwise Exclusive OR (XOR) [^] 238
  Bitwise OR [|] 238
  unary bitwise complement operator [~] 237
  using, for bit manipulation 237, 238
Bitwise OR [|] 238

