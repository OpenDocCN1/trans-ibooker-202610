# 3

# Thread Synchronization Utilities

In this chapter, we will cover the following topics:

+   - Controlling concurrent access to one or more copies of a resource
+   - Waiting for multiple concurrent events
+   - Synchronizing tasks at a common point
+   - Running concurrent-phased tasks
+   - Controlling phase change in concurrent-phased tasks
+   - Exchanging data between concurrent tasks
+   - Completing and linking tasks asynchronously

# Introduction

In Chapter 2, Basic Thread Synchronization, you learned the concepts of synchronization and critical sections. Basically, we talk about synchronization when more than one concurrent task shares a resource, for example, an object or an attribute of an object. The blocks of code that access this shared resource are called critical sections.

If you don't use appropriate mechanisms, you might have incorrect results, data inconsistencies, or error conditions. Therefore, we have to adopt one of the synchronization mechanisms provided by the Java language to avoid these problems.