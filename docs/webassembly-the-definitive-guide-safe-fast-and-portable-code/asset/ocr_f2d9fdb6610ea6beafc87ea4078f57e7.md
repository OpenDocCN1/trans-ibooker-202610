integers. The `Uint8Array` treats each byte as an individual element due to its size definition.

If you check out Figure 4-1 you will hopefully see what is going on. The first element (position 0) of the `Uint32Array` is simply the value 257. This is an interpreted view of the underlying bytes in the `ArrayBuffer`. The `Uint8Array` directly reflects the underlying bytes of the buffer. The bit patterns at the bottom of the diagram reflect the bits per byte for the first two bytes.

![](bbox=[0.18, 0.32, 0.82, 0.78])

Figure 4-1. Representing the value 257.

It may surprise you that there are 1s in the first two bytes. This is due to a confusing notion called endianness that shows up when we store numbers in memory⁵. In this case, a little endian system stores the least significant