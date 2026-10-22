# Chapter 6. Distributed Caching

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 6th chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

Caches exist in many places in an application. The CPUs that run your applications have multi-level, fast hardware caches to reduce relatively slow main memory accesses. Database engines can make use of main memory to cache the contents of the data store in memory so that in many cases queries do not have to touch relatively slow disks.

Distributed caching is an essential ingredient of a scalable system. Caching makes the results of expensive queries and computations available for reuse by subsequent requests at low cost. By not having to reconstruct the cached results for every single request, the capacity of the system is increased, and it is hence able to scale to handle greater workloads.

There’s two flavors of caching that I’ll cover in this chapter. Application caching requires business logic that incorporates the caching and access of precomputed results using distributed caches. Web caching exploits mechanisms built into the HTTP protocol to enable caching of results within the infrastructure provided by the Internet. When used effectively, both will protect your services and databases from heavy read traffic loads.