decremented; if any of these counters reach 0, a new pass of the GC is triggered. Pointer management in blocks comes at a cost, which may be significant. This GC mechanism is not able to free cyclic data structures, that is which reference themselves, as their counters never reach zero: this is shown in Figure 7.3. This phenomenon results in memory leaks.

![](bbox=[0.11, 0.22, 0.89, 0.31])

Figure 7.3. Cycle and reference counter. For a color version of this figure, see www.iste.co.uk/hardin/programming1.zip

# 7.6.3.2. Mark-and-sweep GC

A mark-and-sweep GC goes through the memory graph, marking live blocks. Unmarked blocks are therefore dead and the GC adds them to the free memory, represented by a linked list of free blocks.

![](bbox=[0.3, 0.5, 0.69, 0.6])

Figure 7.4. List of free blocks. For a color version of this figure, see www.iste.co.uk/hardin/programming1.zip

Mark-and-sweep GCs have the ability to free cyclic structures. However, they are not able to merge consecutive free blocks, as each block is reinserted into the list of free blocks independently. This results in fragmentation of the memory, as shown in Figure 7.5, which can prevent the allocation of a structure that is bigger than any individual free block, even if the overall quantity of free memory is sufficient.

# 7.6.3.3. Copying GC

A copying GC uses two memory zones which are equal in size. Only one of these zones is used to allocate memory at any time. When the quantity of available memory becomes insufficient, the GC goes through the memory graph and copies all