

TLAB: gc thread: 0x00007f3c10b8f800 [id: 18519] desired_size: 221KB
  slow allocs: 8  refill waste: 3536B alloc: 0.01613    11058KB
  refills: 73 waste  0.1% gc: 10368B slow: 2112B fast: 0B

The gc in this output means that the line was printed during GC; the thread itself is a regular application thread. The size of this thread’s TLAB is 221 KB. Since the last young collection, it allocated eight objects from the heap (slow allocs); that was 1.6% (0.01613) of the total amount of allocation done by this thread, and it amounted to 11,058 KB. 0.1% of the TLAB being “wasted,” which comes from three things: 10,336 bytes were free in the TLAB when the current GC cycle started; 2,112 bytes were free in other (retired) TLABs, and 0 bytes were allocated via a special “fast” allocator.

After the TLAB data for each thread has been printed, the JVM provides a line of summary data (this data is provided in JDK 11 by configuring the log for tlab*=debug):

```
TLAB totals: thrds: 66  refills: 3234 max: 105
          slow allocs: 406 max 14 waste:  1.1% gc: 7519856B
          max: 211464B slow: 120016B max: 4808B fast: 0B max: 0B
```

In this case, 66 threads performed some sort of allocation since the last young collection. Among those threads, they refilled their TLABs 3,234 times; the most any particular thread refilled its TLAB was 105. Overall, 406 allocations were made to the heap (with a maximum of 14 done by one thread), and 1.1% of the TLABs were wasted from the free space in retired TLABs.

In the per thread data, if threads show many allocations outside TLABs, consider resizing them.

## Sizing TLABs

Applications that spend a lot of time allocating objects outside TLABs will benefit from changes that can move the allocation to a TLAB. If only a few specific object types are always allocated outside a TLAB, programmatic changes are the best solution.

Otherwise—or if programmatic changes are not possible—you can attempt to resize the TLABs to fit the application use case. Because the TLAB size is based on the size of eden, adjusting the new size parameters will automatically increase the size of the TLABs.

The size of the TLABs can be set explicitly using the flag -XX:TLABSize=N (the default value, 0, means to use the dynamic calculation previously described). That flag sets only the initial size of the TLABs; to prevent resizing at each GC, add -XX:-ResizeTLAB (the default for that flag is true). This is the easiest (and, frankly, the only useful) option for exploring the performance of adjusting the TLABs.

When a new object does not fit in the current TLAB (but would fit within a new empty TLAB), the JVM has a decision to make: whether to allocate the object in the heap or whether to retire the current TLAB and allocate a new one. That decision is based on several parameters. In the TLAB logging output, the refill waste value gives the current threshold for that decision: if the TLAB cannot accommodate a new object that is larger than that value, the new object will be allocated in the heap. If the object in question is smaller than that value, the TLAB will be retired.

That value is dynamic, but it begins by default at 1% of the TLAB size—or, specifically, at the value specified by `-XX:TLABWasteTargetPercent=N`. As each allocation is done outside the heap, that value is increased by the value of `-XX:TLABWasteIncrement=N` (the default is 4). This prevents a thread from reaching the threshold in the TLAB and continually allocating objects in the heap: as the target percentage increases, the chances of the TLAB being retired also increases. Adjusting the TLABWasteTargetPercent value also adjusts the size of the TLAB, so while it is possible to play with this value, its effect is not always predictable.

Finally, when TLAB resizing is in effect, the minimum size of a TLAB can be specified with `-XX:MinTLABSize=N` (the default is 2 KB). The maximum size of a TLAB is slightly less than 1 GB (the maximum space that can be occupied by an array of integers, rounded down for object alignment purposes) and cannot be changed.

![](img/1711f142ae3cfd740ef106afbbba2046_206_0.png)

## Quick Summary

- Applications that allocate a lot of large objects may need to tune the TLABs (though often using smaller objects in the application is a better approach).

## Humongous objects

Objects that are allocated outside a TLAB are still allocated within eden when possible. If the object cannot fit within eden, it must be allocated directly in the old generation. That prevents the normal GC life cycle for that object, so if it is short-lived, GC is negatively affected. There’s little to do in that case other than change the application so that it doesn’t need those short-lived huge objects.

Humongous objects are treated differently in G1 GC, however: G1 will allocate them in the old generation if they are bigger than a G1 region. So applications that use a lot of humongous objects in G1 GC may need special tuning to compensate for that.

## G1 GC region sizes

G1 GC divides the heap into regions, each of which has a fixed size. The region size is not dynamic; it is determined at startup based on the minimum size of the heap (the value of Xms). The minimum region size is 1 MB. If the minimum heap size is greater than 2 GB, the size of the regions will be set according to this formula (using log base 2):

```
region_size = 1 << log(Initial Heap Size / 2048);
```

In short, the region size is the smallest power of 2 such that there are close to 2,048 regions when the initial heap size is divided. Some minimum and maximum constraints are in use here too; the region size is always at least 1 MB and never more than 32 MB. Table 6-3 sorts out all the possibilities.

Table 6-3. Default G1 region sizes

| Heap size | Default G1 region size |
|---|---|
| Less than 4 GB | 1 MB |
| Between 4 GB and 8 GB | 2 MB |
| Between 8 GB and 16 GB | 4 MB |
| Between 16 GB and 32 GB | 8 MB |
| Between 32 GB and 64 GB | 16 MB |
| Larger than 64 GB | 32 MB |

The size of a G1 region can be set with the -XX:G1HeapRegionSize=N flag (the default is nominally 0, meaning to use the dynamic value just described). The value given here should be a power of 2 (e.g., 1 MB or 2 MB); otherwise, it is rounded down to the nearest power of 2.

## G1 Region Sizes and Large Heaps

Normally, the G1 GC region size needs to be tuned only to handle humongous object allocation, but it might need to be tuned in one other case.

Consider an application that specifies a very large heap range; -Xms2G -Xmx32G, for example. In that case, the region size will be 1 MB. When the heap is fully expanded, there will be 32,000 G1 GC regions. That is a lot of separate regions to process; the G1 GC algorithm is designed around the idea that the number of regions is closer to 2,048. Increasing the size of the G1 GC region will make G1 GC a little more efficient in this example; select a value so that there will be close to 2,048 regions at the expected heap size.

## G1 GC allocation of humongous objects

If the G1 GC region size is 1 MB and a program allocates an array of 2 million bytes, the array will not fit within a single G1 GC region. But these humongous objects must be allocated in contiguous G1 GC regions. If the G1 GC region size is 1 MB, then to allocate a 3.1 MB array, G1 GC must find four regions within the old generation in which to allocate the array. (The rest of the last region will remain empty, wasting 0.9 MB of space.) This defeats the way G1 GC normally performs compaction, which is to free arbitrary regions based on how full they are.

In fact, G1 GC defines a humongous object as one that is half of the region size, so allocating an array of 512 KB (plus 1 byte) will, in this case, trigger the humongous allocation we’re discussing.

Because the humongous object is allocated directly in the old generation, it cannot be freed during a young collection. So if the object is short-lived, this also defeats the generational design of the collector. The humongous object will be collected during the concurrent G1 GC cycle. On the bright side, the humongous object can be freed quickly because it is the only object in the regions it occupies. Humongous objects are freed during the cleanup phase of the concurrent cycle (rather than during a mixed GC).

Increasing the size of a G1 GC region so that all objects the program will allocate can fit within a single G1 GC region can make G1 GC more efficient. This means having a G1 region size of twice the largest object’s size plus 1 byte.

Humongous allocation used to be a far bigger problem in G1 GC because finding the necessary regions to allocate the object would usually require a full GC (and because such full GCs were not parallelized). Improvements in G1 GC in JDK 8u60 (and in all JDK 11 builds) minimize this issue so it isn’t necessarily the critical problem it sometimes used to be.

![](img/1711f142ae3cfd740ef106afbbba2046_208_0.png)

## Quick Summary

- G1 regions are sized in powers of 2, starting at 1 MB.
- Heaps that have a very different maximum size than initial size will have too many G1 regions; the G1 region size should be increased in that case.
- Applications that allocate objects larger than half the size of a G1 region should increase the G1 region size so that the objects can fit within a G1 region. An application must allocate an object that is at least 512 KB for this to apply (since the smallest G1 region is 1 MB).

## AggressiveHeap

The AggressiveHeap flag (by default, false), was introduced in an early version of Java as an attempt to make it easier to set a variety of command-line arguments—arguments that would be appropriate for a very large machine with a lot of memory

