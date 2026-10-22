![](bbox=[0.13, 0.09, 0.85, 0.72])

Figure 13-3. MongoDB Database Deployment Alternatives

Within each shard, MongoDB stores documents in storage units known as chunks. By default a chunk is a maximum of 64MB. When a chunk grows beyond its maximum configured size, MongoDB automatically splits the