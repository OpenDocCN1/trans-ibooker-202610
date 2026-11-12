The advantage of store-and-forward is that the switch can check the frame for errors before forwarding it on. The downfall is the wait time for the switch to read the entire frame, which is longer than that of the other methods.

# Cut-Through Switching

Cut-through switches were designed to improve the performance of a switch as it relates to processing frames. With cut-through switching, the switch only waits to read the destination MAC address before it begins forwarding the frame. Cut-through switching is faster than store-and-forward switching. Its biggest problem, though, is that the switch may be forwarding bad frames—though the header may be legible, the rest of the frame could be corrupted from a late collision.

# Fragment-Free Switching

Fragment-free switching is a modified form of cut-through switching. Whereas cut-through switching reads up to the destination MAC address field in the frame before making a switching decision, fragment-free switching makes sure that the frame is at least 64 bytes long before switching it (64 bytes is the minimum legal size of an Ethernet frame). The goal of fragment-free switching is to reduce the number of Ethernet runt frames (frames smaller than 64 bytes) that are being switched. Fragment-free switching is sometimes called modified cut-through or runtless switching. With fragment-free switching, a switch could still be switching corrupt frames, because the switch is checking only the first 64 bytes.

![](bbox=[0.14, 0.69, 0.24, 0.75])

**EXAM TIP** For the CCNA exam remember the key points about the switch operation modes. Store-and-forward switching reads the whole frame, checks the FCS, and then forwards the frame. This increases latency for switching but reduces the number of bad frames on the network. Cut-through switching switches a frame as soon as it sees the destination MAC address in the frame. Fragment-free switching will