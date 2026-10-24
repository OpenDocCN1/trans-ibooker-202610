

| 标志 | 作用 | 使用时机 | 另请参阅 |
|---|---|---|---|
| `-XX:+PrintTLAB` | 在GC日志中打印TLAB的摘要信息。 | 使用不支持JFR的JVM时，使用此选项确保TLAB分配高效工作。 | “线程本地分配缓冲区”，第187页 |
| `-XX:TLABSize=N` | 设置TLAB的大小。 | 当应用程序在TLAB之外执行大量分配时，使用此值增大TLAB大小。 | “线程本地分配缓冲区”，第187页 |
| `-XX:-ResizeTLAB` | 禁用TLAB的大小调整。 | 只要指定了TLABSize，请务必禁用此标志。 | “线程本地分配缓冲区”，第187页 |

## 表 A-9. 本地内存跟踪的标志

| 标志 | 作用 | 使用时机 | 另请参阅 |
|---|---|---|---|
| `-XX:NativeMemoryTracking=X` | 启用本地内存跟踪。 | 当需要查看JVM在堆之外使用了哪些内存时。 | “本地内存跟踪”，第252页 |
| `-XX:+PrintNMTStatistics` | 程序终止时打印本地内存跟踪统计信息。 | 当需要查看JVM在堆之外使用了哪些内存时。 | “本地内存跟踪”，第252页 |

## 表 A-10. 线程处理标志

| 标志 | 作用 | 使用时机 | 另请参阅 |
|---|---|---|---|
| `-Xss<N>` | 设置线程本机栈的大小。 | 减小此值可为JVM的其他部分提供更多内存。 | “调优线程栈大小”，第299页 |
| `-XX:-BiasedLocking` | 禁用JVM的偏向锁算法。 | 有助于提升基于线程池的应用程序的性能。 | “偏向锁”，第300页 |

## 表 A-11. 其他JVM标志

| 标志 | 作用 | 使用时机 | 另请参阅 |
|---|---|---|---|
| `-XX:+CompactStrings` | 尽可能使用8位字符串表示（仅Java 11）。 | 默认启用；始终使用。 | “紧凑字符串”，第363页 |
| `-XX:-StackTraceInThrowable` | 禁止在抛出异常时收集堆栈跟踪。 | 在异常频繁抛出且堆栈极深的系统上（且无法通过修复代码来减少异常抛出时）。 | “异常”，第386页 |
| -XXshare | 控制类数据共享。 | 使用此标志为应用程序代码创建新的CDS存档。 | “类数据共享”，第377页 |

## 表 A-12. Java Flight Recorder 标志

| 标志 | 作用 | 使用时机 | 另请参阅 |
|---|---|---|---|
| -XX:+FlightRecorder | 启用Java Flight Recorder。 | 始终建议启用Flight Recorder，因为它开销很小，除非正在实际录制（此时开销因所用功能而异，但仍相对较小）。 | “Java Flight Recorder”，第74页 |
| -XX:+FlightRecorderOptions | 通过命令行设置默认录制的选项（仅Java 8）。 | 控制JVM如何进行默认录制。 | “Java Flight Recorder”，第74页 |
| -XX:+StartFlightRecorder | 使用给定的Flight Recorder选项启动JVM。 | 控制JVM如何进行默认录制。 | “Java Flight Recorder”，第74页 |
| -XX:+UnlockCommercialFeatures | 允许JVM使用商业（非开源）功能。 | 如果拥有相应的许可证，在Java 8中需要设置此标志才能启用Java Flight Recorder。 | “Java Flight Recorder”，第74页 |

## 索引

## 符号

- -client, 413
- -server, 413
- -verbose:gc, 148, 417
- -Xlog:gc*, 148, 417
- -Xlog:gc+stringdedup*=debug, 366
- -Xlog:gc<path>, 148, 417
- -Xmn, 142, 416
- -Xms, 139, 158, 415
- -Xmx, 139, 158, 200, 415
- -Xshare, 421
- -Xss<N>, 421
- -XX:+AllocatePrefetchLines=<N>, 62
- -XX:+AlwaysTenure, 184
- -XX:+BackgroundCompilation, 108
- -XX:+CMSIncrementalMode, 419
- -XX:+CompactStrings, 364, 421
- -XX:+DisableExplicitGC, 129, 417
- -XX:+DoEscapeAnalysis, 110, 414
- -XX:+DumpLoadedClassList=filename, 378
- -XX:+EnableJVMCI, 115
- -XX:+ExitOnOutOfMemoryError, 214
- -XX:+FlightRecorder, 82, 422
- -XX:+FlightRecorderOptions, 84-85, 422
- -XX:+HeapDumpOnOutOfMemoryError, 212, 420
- -XX:+NeverTenure, 184
- -XX:+OptimizeStringConcat, 371
- -XX:+PrintAdaptiveSizePolicy, 143, 416
- -XX:+PrintAOT, 118
- -XX:+PrintCompilation, 96, 98, 118, 414
- -XX:+PrintFlagsFinal, 61
- -XX:+PrintGC, 148, 417
- -XX:+PrintGCDateStamps, 148, 418
- -XX:+PrintGCDetails, 148, 417
- -XX:+PrintGCTimeStamps, 148, 417
- -XX:+PrintInlining, 109
- -XX:+PrintNMTStatistics, 252, 421
- -XX:+PrintReferenceGC, 418
- -XX:+PrintStringDeduplicationStatistics, 366
- -XX:+PrintStringTableStatistics, 369
- -XX:+PrintTenuringDistribution, 184, 416
- -XX:+PrintTLAB, 421
- -XX:+ScavengeBeforeFullGC, 195
- -XX:+StartFlightRecorder, 84, 422
- -XX:+StringTableSize=N, 367, 420
- -XX:+TieredCompilation, 413
- -XX:+UnlockCommercialFeatures, 82, 422
- -XX:+UnlockExperimentalVMOptions, 115, 197, 200
- -XX:+UseAdaptiveSizePolicy, 143, 184, 416
- -XX:+UseCMSInitiatingOccupancyOnly, 181, 419
- -XX:+UseCompressedOops, 247, 420
- -XX:+UseConcMarkSweepGC, 128, 415
- -XX:+UseEpsilonGC, 200, 415
- -XX:+UseG1GC, 128, 146, 414
- -XX:+UseGCLogFileRotation, 148, 418
- -XX:+UseGCOverheadLimit, 214
- -XX:+UseJVMCICompiler, 115
- -XX:+UseLargePages, 261, 420
- -XX:+UseParallelGC, 127, 146, 414
- -XX:+UseParallelOldGC, 127
- -XX:+UseParNewGC, 128, 415
- -XX:+UseSerialGC, 127, 414
- -XX:+UseShenandoahGC, 197, 415
- -XX:+UseStringDeduplication, 365, 419
- -XX:+UseTransparentHugePages, 264
- -XX:+UseZGC, 197, 415
- -XX:-AggressiveHeap, 193-195, 417
- -XX:-BiasedLocking, 421
- -XX:-EnableContended, 298
- -XX:-Inline, 109
- -XX:-ResizeTLAB, 190, 421
- -XX:-StackTraceInThrowable, 389, 421
- -XX:-TieredCompilation, 94
- -XX:-UseBiasedLocking, 300
- -XX:-UseTLAB, 188
- -XX:AOTLibrary=<path>, 414
- -XX:CICompilerCount=<N>, 107, 414
- -XX:CMSInitiatingOccupancyFraction=N, 180-182, 419
- -XX:CompileThreshold=<N>, 106, 414
- -XX:ConcGCThreads=N, 172, 182, 418
- -XX:ErgoHeapSizeLimit=N, 196
- -XX:G1HeapRegionSize=N, 419
- -XX:G1MixedGCCountTarget=N, 173, 419
- -XX:GCLogFileSize=N, 148, 418
- -XX:GCTimeLimit=<N>, 214, 420
- -XX:GCTimeRatio=N, 158-160, 173, 418
- -XX:HeapDumpAfterFullGC, 212
- -XX:HeapDumpBeforeFullGC, 212
- -XX:HeapDumpPath=<path>, 212, 420
- -XX:HeapFreeLimit=<N>, 214, 420
- -XX:InitialCodeCacheSize=<MB>, 95, 413
- -XX:InitialRAMFraction=N, 196
- -XX:InitialSurvivorRatio=N, 183, 416
- -XX:InitialTenuringThreshold=N, 184, 417
- -XX:InitiatingHeapOccupancyPercent=N, 172, 419
- -XX:MaxDirectMemorySize=N, 259, 420
- -XX:MaxFreqInlineSize=N, 110
- -XX:MaxGCPauseMillis=N, 158-160, 171, 173, 180, 418
- -XX:MaxInlineSize=N, 110
- -XX:MaxMetaspaceSize=N, 416
- -XX:MaxNewSize, 142, 416
- -XX:MaxRAM=N, 196
- -XX:MaxRAMFraction=N, 196
- -XX:MaxTenuringThreshold=N, 184, 417
- -XX:MetaspaceSize=N, 416
- -XX:MinRAMFraction=N, 196
- -XX:MinSurvivorRatio=N, 183, 416
- -XX:MinTLABSize=N, 191
- -XX:NativeMemoryTracking=X, 252, 254, 421
- -XX:NewRatio, 142, 415
- -XX:NewSize, 142, 415
- -XX:NewSize=N, 196
- -XX:NumberOfGCLogFiles=N, 148, 418
- -XX:OldSize=N, 196
- -XX:ParallelGCThreads=N, 146, 172, 416
- -XX:ReservedCodeCacheSize=<MB>, 95, 413
- -XX:SharedArchiveFile, 379
- -XX:SoftRefLRUPolicyMSPerMB=N, 236, 420
- -XX:StringDeduplicationAgeThreshold=N, 367
- -XX:TargetSurvivorRatio=N, 184, 416
- -XX:Tier3InvocationThreshold=N, 106
- -XX:Tier4InvocationThreshold=N, 106
- -XX:TLABSize=N, 190, 421
- -XX:TLABWasteIncrement=N, 191
- -XX:TLABWasteTargetPercent=N, 191
- -XX:UseAVX=<N>, 112, 414
- @Contended 注解, 298

A
自适应大小调整, 143-144
高级编译器标志
  编译线程, 107-109
  编译阈值, 105-107
特定于CPU的代码, 112
逃逸分析, 110
内联, 109
高级向量扩展 (AVX2), 112
提前（AOT）编译, 116-118
分配
  巨型对象, 191
  大对象, 186-193
  TLAB大小调整, 190
  线程本地分配缓冲区, 187-190
α值, 31
阿姆达尔定律, 287
匿名类, 397-399
匿名类加载, 399
AOT（提前）编译, 116-118
异步分析器, 66
异步REST服务器, 311-314
异步数据库调用, 321
异步HTTP, 315-322
  客户端, 317-320
  客户端与线程使用, 320
异步出站调用, 314-322
原子类, 293
自动并行化, 285-286
AVX2（高级向量扩展）, 112

