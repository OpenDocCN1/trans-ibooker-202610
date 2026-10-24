Tiered Compilation Trade-offs 113

The GraalVM 115

Precompilation 116

Ahead-of-Time Compilation 116

GraalVM Native Compilation 118

Summary 120

# 5. An Introduction to Garbage Collection. 121

Garbage Collection Overview 121

Generational Garbage Collectors 123

GC Algorithms 126

Choosing a GC Algorithm 130

Basic GC Tuning 138

Sizing the Heap 138

Sizing the Generations 141

Sizing Metaspace 144

Controlling Parallelism 146

GC Tools 147

Enabling GC Logging in JDK 8 148

Enabling GC Logging in JDK 11 148

Summary 152

# 6. Garbage Collection Algorithms. 153

Understanding the Throughput Collector 153

Adaptive and Static Heap Size Tuning 156

Understanding the G1 Garbage Collector 160

Tuning G1 GC 170

Understanding the CMS Collector 174

Tuning to Solve Concurrent Mode Failures 179

Advanced Tunings 182

Tenuring and Survivor Spaces 182

Allocating Large Objects 186

AggressiveHeap 193

Full Control Over Heap Size 195

Experimental GC Algorithms 197

Concurrent Compaction: ZGC and Shenandoah 197

No Collection: Epsilon GC 200

Summary 201

# 7. Heap Memory Best Practices. 203

Heap Analysis 203

Heap Histograms 204