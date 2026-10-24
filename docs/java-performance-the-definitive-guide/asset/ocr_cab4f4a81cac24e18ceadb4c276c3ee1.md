large pages
  Linux huge pages, 262
  Linux transparent huge pages, 263
  Windows, 264
lazy initialization, 219-223
  eager deinitialization, 222
  runtime performance, 219
lazy loading, 355
lazy traversal, 399-402
Linux
  huge pages, 262
  memory leaking, 260
  transparent huge pages, 263
load generators, 29
lock inflation, 80
locks
  biased locking, 300
  monitoring, 301-306
  transactions, 340-344
logging
  and Java SE API, 390-392
  enabling GC logging in JDK 11, 148-151
  enabling GC logging in JDK 8, 148
  GC logs and reference handling, 235
low-pause garbage collectors, 125
lukewarm compilation, 106

M
macrobenchmarks, 20-21
main memory, registry values versus, 92
MALLO​C_ARENA_MAX, 260
marshalling, 323
mat, 206
memory, 95
  (see also native memory)
  collection memory sizes, 396
  collections and memory efficiency, 395
  flags for management, 420
  heap (see heap memory)
  nonheap (see native memory)
  reserved versus allocated, 95
  using less, 215-225
Memory Analyzer Tool (mat), 206
memory leaks
  classloader, 145
  collection class as cause of, 213
  Linux, 260
  NMT and, 256
mesobenchmarks, 22

metadata (class), 377-380
Metadata GC Threshold, 170
metaspace
  out-of-metaspace-memory error, 211
  sizing, 144-146
microbenchmarks, 15-20, 17
minor GC, 124
mixed garbage collection, 166-169
mode failures, 179-182
monitoring
  blocked thread visibility, 302-306
  thread visibility, 301
  threads/locks, 301-306
mutator threads, 123
multicore hardware, 5

N
native memory, 249-265
  flags for tracking, 421
  footprint, 249-260
  JVM tunings for OS, 261-265
  Linux system memory leaking, 260
  MALLOC_ARENA_MAX, 260
  NMT, 252-256
  out-of-native-memory error, 210
  shared library native memory, 256-260
Native Memory Tracking (NMT), 252-256
  auto disabling, 256
  detailed tracking information, 254
  memory allocations over time, 255
native profilers, 72-73
netstat, 57
network usage, monitoring, 57
nicstat, 57
NIO buffers, 258
NMT (see Native Memory Tracking)
nonblocking I/O (NIO)
  native NIO buffers, 258
  overview, 307-309
nonheap memory (see native memory)
NoSQL databases, 329, 360
not-entrant code, 102-104
null hypothesis, 31

O
object alignment, object size and, 217
object life-cycle management, 225-248
  compressed oops, 246
  object reuse, 225-231