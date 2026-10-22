![](bbox=[0.13, 0.08, 0.88, 0.75])

# Figure 12-4. Log Replication with Raft

Only a majority of followers are required to commit an entry on the log. This means the committed log entries may not be identical at every follower at any instant. If a follower falls behind or is partitioned, and is not acknowledging AppendEntries requests, the leader continues to resend