# Data ownership
and data storage

# This chapter covers

+   - Which data microservices store
+   - Understanding how data ownership follows business capabilities
+   - Using data replication for speed and robustness
+   - Building read models from event feeds with event subscribers
+   - Implementing data storage in microservices

Software systems create, use, and transform data. Without the data, most software systems wouldn’t be worth much, and that’s true for microservice systems, too. In this chapter, you’ll learn where a piece of data should be stored and which microservice should be responsible for keeping it up to date. Furthermore, you’ll learn how you can use data replication to make your microservice system both more robust and faster.

# 6.1 Each microservice has a data store

One of the characteristics of microservices identified in chapter 1 is that each microservice should own its data store. The data in that data store is solely under