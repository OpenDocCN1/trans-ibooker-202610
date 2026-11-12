sure the data is exactly what you need, you may as well just make your own job and your own copy for safety's sake.

Enforcing access controls on the source can help clear up who can access what data, but only from the primary source. But restricting data access can backfire. A lengthy or cumbersome process to gain access can result in people simply making copies from other less protected sources. In Figure 1-7, notice that the Predictions domain simply circumnavigates the source access controls by copying the sales data from the User insights domain.

But are these copies _really_ the same data? Synchronization frequency, transformations, time zones, intermittent failures, and an incorrect environment (e.g., staging, not production) are just a few issues that can affect the integrity of your copied source. It's possible that you _think_ you're getting the correct data, but you're not and you don't know it. For example, you copy one data set thinking it's synced to UTC-0 time, but it's actually synced to UTC-6. The format, partitioning, and ordering of data may appear identical, yet these hard-to-detect, undocumented differences still remain.

Custom point-to-point connections can be a challenge to maintain, cause a sprawl of data, and can result in many duplicate sync jobs that produce similar yet different data.

This disjointed model and responsibilities of data ownership and distribution lead to _bad data_, which is costly in terms of time, money, and missed opportunities. Let's take a look at the costs, before wrapping up with why data mesh purports to solve these issues.

# Bad Data: The Costs of Inaction

Bad data typically goes undetected until it is applied to a schema. For example, you can't insert TEXT into an Int32 column in a database. By using schema on read, however, we effectively defer validating our data until the end of the data pip­ ing process. And while our _sources_ may use a schema, there's no guarantee that our countless point-to-point pipelines have correctly captured the schema alongside the data. There have been many an Int32 captured as an String, or, at worst, an Object.

Bad data is costly to fix, and it's more costly the more widespread it is. Everyone who has accessed, used, copied, or processed the data may be affected and may require mitigating action on their part. The complexity is further increased by the fact that not every consumer will “fix” it in the same way. This can lead to divergent results that are divergent with others and can be a nightmare to detect, track down, and rectify.