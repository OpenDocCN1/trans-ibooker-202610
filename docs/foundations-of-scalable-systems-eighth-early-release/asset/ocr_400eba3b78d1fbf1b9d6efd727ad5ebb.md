and benefit from fast reads and writes. Or, if you want to balance performance and consistency, there’s another option, as I’ll explain in the next section.

# Quorum Reads and Writes

There’s an option that lies between the alternatives discussed in the previous section. These are known as quorum reads and writes. Quorum simply means the majority, which is (N/2)+1.⁶ For our 3 replicas, the majority is 2. For 5 replicas, the majority is 3, and so on.

If we configure both the W and R value to be the quorum, we can balance the performance of reads and writes and still provide access to the latest updated value of a data object. Figure 11-2 illustrates how quorums work. With three replicas, a quorum means a write must succeed at 2 replicas, and a read must access two replicas. Initially all three replicas have a data object K with value v1, and the following sequence of actions takes place:

+   1. Client1 updates the object to hold value v2, and the write is acknowledged as successful once a quorum, in this case Replica-1 and Replica-2, are updated.
+   2. The command to update to Replica-3 is delayed (slow network? busy node?).
+   3. Client2 issues a read on object K.
+   4. Replica-2 acts as the request coordinator and sends a read request to the other two replicas for their value for K. Replica-3 is first to respond with K=v1.
+   5. Replica-2 compares its value for K with that returned from Replica-3, and determines that v2 is the most recently updated value. It returns value v2 to Clients2.