## 索引

### A

AbstractQueuedSynchronizer 类
  tryAcquire() 方法 396
  tryRelease() 方法 396
action
  应用于流的元素 266, 268, 269
asynchronous stream
  实现 421, 422, 424, 426, 427, 428
atomic arrays
  使用 337, 338, 340
atomic object
  实现 409, 412
atomic variables
  关于 409
  使用 330, 331, 335
  使用，代替同步 519, 520, 521
attributes, Thread class
  ID 9
  name 9
  priority 9
  status 9

### B

big data sets
  处理，流使用 545, 546, 549
block of code
  与 Lock 同步 66, 67, 69, 70, 71
blocking collections 296
blocking data structure 306
blocking data structures 531
blocking operations
  关于 540
  使用，避免在锁内 540, 541
blocking thread-safe dequeues
  使用 302, 304
blocking thread-safe queue ordered

### C

using, by priority 305, 307, 309
blocking thread-safe queue
  用于与生产者/消费者通信 496, 497, 499, 500
broken CyclicBarrier objects 117
characteristics, Spliterator
  CONCURRENT 418
  DISTINCT 418
  IMMUTABLE 418
  NONNULL 418
  ORDERED 418
  SIZED 418
  SORTED 418
  SUBSIZED 418
checked exceptions 33, 226
collections
  关于 296
  blocking collections 296
Collectors class
  averagingDouble() 方法 266
  averagingInt() 方法 266
  averagingLong() 方法 266
  groupingByConcurrent() 方法 263
  joining() 方法 263
  partitioningBy() 方法 263
  toCollection() 方法 266
  toConcurrentMap() 方法 263
  toList() 方法 266
compare-and-swap operation 338
CompletableFuture class
  关于 96
  anyOf() 方法 148
  cancel() 方法 148
  completeAsync() 方法 148
  completeExceptionally() 方法 148
getNow() 方法 150
runAfterBothAsync() 方法 149
runAfterEitherAsync() 方法 149
runAsync() 方法 148
thenAcceptBothAsync() 方法 149
thenCombineAsync() 方法 149
thenComposeAsync() 方法 149
CompletionService class
  poll() 方法 197
  take() 方法 197
concurrency code
  测试，使用 MultithreadedTC 473
concurrent access
  控制，对一个或多个资源副本的访问 97, 98, 99, 102
concurrent code
  分析，使用 FindBugs 457, 458, 459, 460, 461, 462
concurrent data structures
  blocking data structures 531
  使用，代替编程 530, 531, 532
concurrent phased tasks
  阶段变化，在中控制 128, 129, 132, 133, 134
  运行 118, 119, 120, 123, 125
concurrent random numbers
  生成 509, 510
concurrent tasks
  数据，在之间交换 135, 136, 137, 138, 139
ConcurrentHashMap
  computeIfPresent() 方法 329
  forEach() 方法 328
  forEachEntry() 方法 328
  forEachKey() 方法 329
  forEachValue() 方法 329
  getOrDefault() 方法 329
  merge() 方法 329
  reduce() 方法 328
  reduceEntries() 方法 329
  reduceKeys() 方法 329
  reduceValues() 方法 329
  reduceXXXToDouble() 方法 329
  reduceXXXToInt() 方法 329
  reduceXXXToLong() 方法 329
search() 方法 328
searchEntries() 方法 329
searchKeys() 方法 329
searchValues() 方法 329
ConcurrentLinkedDeque class
  getFirst() 方法 301
  getLast() 方法 301
  peek() 方法 301
  peekFirst() 方法 301
  peekLast() 方法 301
  remove() 方法 301
  removeFirst() 方法 301
  removeLast() 方法 301
ConcurrentNavigableMap interface
  key 317
  value 317
ConcurrentSkipListMap class
  headMap(K toKey) 方法 322
  pollLastEntry() 方法 322
  putIfAbsent(K key, V Value) 方法 322
  replace(K key, V Value) 方法 322
  tailMap(K fromKey) 方法 322
conditions
  使用，在同步代码中 60, 61, 62, 63, 64, 65
  验证，在流的元素中 283, 284, 285
CountDownLatch class
  关于 96
  await() 方法 108
  basic elements 107
CountedCompleter class
  方法 225
critical section 50, 431
critical sections 95
custom Lock class
  实现 391, 393, 394, 395
CyclicBarrier class
  关于 96
  await() 方法 117
CyclicBarrier object
  重置 117
D
daemon thread
  关于 28
creating 29, 32
running 29, 32
data access
  同步，使用读写锁 73, 74, 76, 78
data structure 295
data structures, Java API
  ArrayBlockingQueue 531
  ConcurrentHashMap 532
  ConcurrentLinkedDeque 531
  ConcurrentLinkedQueue 531
  ConcurrentSkipListMap 532
  DelayQueue 531
  LinkedBlockingDeque 531
  LinkedBlockingQueue 531
  LinkedTransferQueue 532
  PriorityBlockingQueue 532
  SynchronousQueue 532
data
  交换，在并发任务之间 135, 136, 137, 138, 139
deadlocks
  避免 72
  避免，通过排序死锁 516, 517, 518
delayed elements
  线程安全列表，使用 311, 312, 315, 316
delayed tasks 371
DelayQueue class
  clear() 方法 317
  offer (E e) 317
  peek() 方法 317
  take() 方法 317
deprecated methods
  destroy() 543
  resume() 543
  stop() 543
  suspend() 543
  使用，避免 542
DocumentTask 216
E
Eclipse
  配置，用于调试并发代码 463, 464, 466
effective log messages
writing 451, 453, 454, 455, 456
elements of stream
  action，应用于 266, 268, 269
  conditions，验证在 283, 284, 285
elements
  收集，流的 258, 260, 262
  过滤，流的 270, 271
  归约，流的 252, 254, 255
  排序，流的 279, 280, 282
  转换，流的 274, 275, 276, 277
exceptions
  checked exceptions 226
  抛出，在任务中 226, 227, 230
  unchecked exceptions 226
Exchanger class 96
Executor framework
  关于 152, 198, 545
  监控 440, 441
  结果，处理 Runnable 对象的 483, 484, 485, 486, 488
Executor object
getActiveCount() 方法 443
getCompletedTaskCount() 方法 443
getCorePoolSize() 方法 443
getPoolSize() 方法 443
getTaskCount() 方法 443
isShutdown() 443
isTerminated() 方法 443
isTerminating() 方法 443
ThreadFactory，在中使用 368, 369, 370
executor
  任务，在中取消 184, 185, 186
  线程管理，委托给 527
Executors class
  newCachedThreadPool() 方法 159
  newSingleThreadExecutor() 方法 159
executors
  使用，代替线程组 544
F
factory pattern
  关于 45
  线程，通过创建 46, 47
fair mode 66
FindBugs


concurrent code, analyzing with 457, 458, 459, 460, 461, 462
download link 458
fork/join framework
about 198, 199, 545
DocumentTask 216
ForkJoinPool class 200
ForkJoinTask class 200
LineTask 217
operations 199
results of tasks, joining 208, 209, 210, 211, 212, 213, 214, 215
tasks, limitations 200
using, instead of executors 536
fork/join pool
creating 201, 202, 206
monitoring 444, 445, 447, 448
ForkJoinPool class
getActiveThreadCount() method 448
getParallelism() method 448
getPoolSize() method 448
getQueuedSubmissionCount() method 448
getQueuedTaskCount() method 448
getRunningThreadCount() method 448
getStealCount() method 448
hasQueuedSubmissions() method 448
isTerminated() method 448
methods 207
uncontrolled exceptions, processing in 490, 492, 493, 494
ForkJoinTask class
exec() method 391
getRawResult() method 390
setRawResult() method 390
Future interface
get(long timeout, TimeUnit unit) method 164
G
get() method
versus join() method 231
H
hash table 323
high-level mechanisms, for synchronizing multiple threads
CompletableFeature class 96
CountDownLatch class 96
CyclicBarrier class 96
Exchanger class 96
Phaser class 96
semaphores 96
I
immutable objects
about 513
using 514
intermediate operations, streams
about 258
interruption, of thread
controlling 19, 20, 21, 22, 23
J
Java API
data structures 531
Java Collections framework 295
Java Concurrency API 353, 545
Java
mechanisms, for creating streams 414
JConsole
about 477
monitoring with 478, 480, 481
join() method
versus get() method 231
L
lazy initialization
about 533
using 534, 535
LineTask 217
LinkedBlockingDeque class
add() method 305
addFirst() method 305
addLast() method 305
getFirst() method 305
getLast() method 305
peek() method 305
peekFirst() method 305
peekLast() method 305
poll() method 305
pollFirst() method 305
pollLast() method 305
takeFirst() method 305
takeLast() method 305
LinkedTransferQueue class
methods 500
LinkedTransferQueue data structure 398
Lock interface
monitoring 431, 432, 433, 435
lock() operation 391
Lock
block of code, synchronizing with 66, 67, 69, 70, 71
multiple conditions, using in 78, 79, 80, 81, 82, 84, 85, 86
locks
holding 523, 526
log system
about 451
considerations 457
Logger
components 451
LogRecord class
getLevel() method 456
getMessage() method 456
getMillis() method 456
getSourceClassName() method 456
getSourceMessageName() method 456
LongAdder class
add() method 337
decrement() method 337
reset() method 337
M
MapReduce
about 251
Map operation 251
Reduce operation 251
method
synchronizing 51, 52, 56, 59
methods, for filtering elements in stream
distinct() 273
dropWhile() 274
filter() 273
limit() 273
skip() 273
takeWhile() 274
methods, LinkedTransferQueue class
getWaitingConsumerCount() 500
hasWaitingConsumer() 500
offer(E e) 500
peek() 501
poll(long timeout, TimeUnit unit) 501
methods, Semaphore class
availablePermits() 508
getQueueLength() 508
hasQueuedThreads() 508
isFair() 508
methods, Thread class
activeCount() 505
dumpStack() 505
getId() 504
getName() 504
getPriority() 504
getStackTrace() 504
getState() 504
getThreadGroup() 504
metronome 473
modes, StampedLock locks
Optimistic Read 87
Read 87
Write 87
multiple concurrent events
waiting for 103, 105, 106, 107
multiple conditions
using, in Lock 78, 79, 80, 81, 82, 84, 85, 86
multiple tasks
running 165, 166, 167, 168, 169, 171, 174, 175
MultithreadedTC library
download link 473
MultithreadedTC
concurrency code, testing with 473
MyLock class
getOwnerName() method 434
getThreads() method 435
N
NetBeans
configuring, for debugging concurrency code 467, 468, 470, 471
O
P
R
S
non-blocking collections 296
non-blocking thread-safe dequeues
using 296, 297, 300
non-fair mode 66, 103
operations, fork/join framework
fork operation 199
join operation 199
Parallel Streams 200
parallelism 8
periodic tasks 371
phase change
controlling, in concurrent phased tasks 128, 129, 132, 133, 134
Phaser class
about 96
arrive() method 126
awaitAdvance(int phase) method 127
awaitAdvanceInterruptibly(int phaser) method 127
bulkRegister(int Parties) method 127
monitoring 436, 437, 439
register() method 127
Phaser object
active state 125
phaser object
getArrivedParties() method 439
getPhase() method 439
getRegisteredParties() method 439
getUnarrivedParties() method 439
Phaser object
participants, registering in 127
termination state 126
termination, forcing of 127
priority-based Executor class
implementing 359, 360, 361, 362
PriorityBlockingQueue class
clear() method 311
peek() method 311
put(E e) method 311
take() method 311
PriorityBlockingQueue data structure 398
producer/consumer problem 495
race conditions 50
reactive programming
with reactive streams 288, 289, 292, 293, 294
reactive streams
about 287, 421
elements 287
specification 287
read/write locks
data access, synchronizing with 73, 74, 76, 78
ReentrantLock class
getHoldCount() method 435
getQueueLength() method 435
hasQueuedThreads() method 435
isFair() method 435
isHeldByCurrentThread() method 435
isLocked() method 435
scheduled thread pool 371
semaphore 505
Semaphore class
acquireUninterruptibly() method 102
methods 508
monitoring 505
tryAcquire() method 102
tryAcquire(long timeout, TimeUnit unit) method 102
semaphores
about 96
concept of fairness 103
singleton classes 533
Skip List
about 317
reference 317
sources
streams, creating from 242, 245, 246, 247, 248, 249, 250
Spliterator interface
characteristics() method 419
estimatedSize() 419
tryAdvance() method 419
trySplit() method 419
StampedLock class
about 87
advanced locking 88, 90, 91, 92
methods 87, 93, 94
StampedLock locks
features 87
modes 87
static code analysis tool 457
Stream class
allMatch() method 285
anyMatch() method 286
average() method 250
count() method 249
findAny() method 286
findFirst() method 286
flatMap() method 278
flatMapToDouble() method 278
forEach() method 249
limit() method 249
map() method 278
mapToDouble() method 278
mapToInt() method 278
mapToLong() method 278
noneMatch() method 286
parallel() method 250
peek() method 250
stream generator
implementing 414, 415, 416
Streams class
flatMapToInt() method 278
flatMapToLong() method 278
streams
about 414
creating, from different sources 242, 245, 246, 247, 248, 249, 250
elements, collecting of 258, 260, 262
elements, filtering of 270, 271
elements, reducing of 252, 254, 255
elements, sorting of 279, 280, 282
elements, transforming of 274, 275, 276, 277
monitoring 449
used, for processing big data sets 545, 546, 549
synchronization 95
synchronized code
conditions, using in 60, 61, 62, 63, 64, 65
T
Task object
entering() method 456
exiting() method 456
log() method 456
tasks
canceling 232, 233, 235, 237, 238
canceling, in executor 184, 185, 186
completing, asynchronously 140, 141, 145, 146, 147
controlling, finished in executor 187, 188, 190
customization, running in fork/join framework 386, 387, 388, 389
customization, running in scheduled thread pool 371, 372, 374, 376, 377
delayed tasks 371
exceptions, throwing in 226, 227, 230
executing, in executor 160, 161, 162, 163
launching of tasks, separating 191, 192, 193, 195, 196
linking, asynchronously 140, 141, 145, 146, 147
periodic tasks 371
results, processing in executor 191, 192, 195, 196
running, asynchronously 218, 219, 221, 222, 224
running, in executor after delay 176, 177
running, in executor periodically 179, 181, 183
synchronizing, in common point 108, 109, 110, 111, 112, 113, 114, 115, 116
terminal operations, streams 258
Thread class
attributes 9
methods 504
monitoring 501, 503, 504
thread executor
creating 152, 154, 156, 157, 158
rejected tasks, controlling 152, 154, 156, 157, 158
thread local variables
using 36, 37, 38, 39
thread management
delegating, to executors 527
thread-safe 530
thread-safe HashMaps

using 323, 327
线程安全的列表
    使用，带延迟元素 311, 312, 315, 316
线程安全的可导航映射
    使用 319, 320, 321
ThreadFactory 接口
    实现，以生成自定义线程 363, 366, 368
    实现，以为 fork/join 框架生成自定义线程 379, 381, 385
ThreadFactory
    使用，在 Executor 对象中 368, 369, 370
ThreadGroup 类
    resume() 方法 543
    stop() 方法 543
    suspend() 方法 543
ThreadPoolExecutor 类
    awaitTermination(long timeout, TimeUnit unit) 方法 159
    自定义 353, 354, 357, 358
    isShutdown() 159
    isTerminated() 方法 159
    shutdownNow() 方法 159
线程
    关于 8
    特征，设置 9, 10, 11, 14
    创建 8, 9, 10, 11, 14
    创建，通过工厂模式 45, 47
    分组 41, 42, 43
    中断 16, 18
线程中断，控制 19, 20, 21, 22, 23
恢复 23
运行 10, 11, 14
休眠 23
未受控异常，在中处理 32, 33, 35, 36
等待，线程结束 26, 28
传输队列
    实现，基于优先级 398, 400, 404, 407, 408

## U
未检查异常 33, 226
未受控异常
    处理，在 ForkJoinPool 类中 490, 492, 493, 494
    处理，在线程组中 41, 42, 43
    处理，在线程中 32, 33, 35, 36
unlock() 操作 391

## V
变量句柄
    使用 347
VisualVM
    参考 482
volatile 关键字
    使用 342, 343

## W
工作线程 199
