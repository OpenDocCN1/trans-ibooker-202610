Setting the API does change the way that you interact with the database,
but it does not change the underlying data. At the time of writing, it is not
possible to change this selection once you have made it however; it would
appear that the functionality to switch this API selection after creation is
something that Microsoft would like to introduce in the future.

This is the second edition of this book, and, in the first edition, this chapter dealt with
interfacing with MongoDB. By selecting a MongoDB API, we should be able to use the
exact same code and simply switch the database.

# Location

Location is a much bigger topic than we can cover here; however, essentially you are
selecting the place where your resource will be physically located. Because information
travels at a finite speed, you should consider this selection carefully; however, Cosmos does
allow the opportunity to replicate the data in other physical locations. So if, as I am, you are
located in the United Kingdom, you might set the location there. But if, say, 30% of your
user base is located in Australia, then, with Cosmos, you can have your data replicated in
that region, reducing latency.

# Geo-redundancy/multi-region writes

The geo-redundancy and multi-region write settings relate to Cosmos's ability to store data
in multiple physical locations simultaneously. Both are very big topics, but the basic idea is
simple: make your data geo-redundant if it is important that your data is safe, and enable
multi-region writes if you wish to lower your latency time and have access from multiple
physical locations across the globe.

> As is the case with all Azure services, these services have a cost associated
> with them. The billing model is relatively complex because the services
> are complex. If you are concerned about cost, Microsoft provides a
> calculator (https://azure.microsoft.com/en-gb/pricing/calculator)
> that will give you an idea of how much any given service might cost.