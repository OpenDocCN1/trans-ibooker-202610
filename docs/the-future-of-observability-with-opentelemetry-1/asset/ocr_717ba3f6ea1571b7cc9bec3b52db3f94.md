# What is Solution-Specific Instrumentation?

In this chapter, the term *solution-specific instrumentation* refers to any instrumentation designed to work with a specific observability system, using a client developed as an outgrowth of that particular system’s data storage system. In these systems, the clients and storage systems often become deeply intertwined. As a result, a complete re-instrumentation is usually required for an application to switch from one observability system to another.

Solution-specific instrumentation is a holdover from the vertical integration inherent to the “three pillars.” Every backend ingests a specific type of proprietary data; ergo, the creators of those backends must also supply the instrumentation that generates that data.

This approach to instrumentation creates trouble for everyone involved in software development: the providers, the users, and the OSS library authors.

# Observability Is Drowning in Solution-Specific Instrumentation

From the perspective of an observability system, instrumentation represents an immense amount of overhead.

In the past, internet applications were fairly homogenous, and it was possible to center an observability system around a particular web framework: Java Spring, Ruby on Rails, or .NET. But over time, the diversity of software has exploded. Maintaining instrumentation for every popular web framework and database client now represents an immense investment.

The duplication of effort that results is simply enormous. Traditionally, vendors used their investment in instrumentation as both a selling point and a form of gatekeeping. But the increasing rate of software development is beginning to make this untenable. The surface area is simply too great for a reasonably sized instrumentation team to keep up with.

This burden is especially grim for new, novel observability systems, especially OSS observability projects. If a novel system cannot be