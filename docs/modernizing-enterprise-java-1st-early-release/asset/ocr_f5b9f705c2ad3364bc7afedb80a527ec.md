# NOTE

When we skaffolded the project at the beginning of this chapter, we included a series of dependencies like Panache and we use it to map our Data Model as Entity into a database.

We can also see that our app is up and running, listening to port 8080. If you open your browser now at http://localhost:8080 you will see a Quarkus Welcome page (as in Figure 2-4)

![](bbox=[0.14, 0.3, 0.86, 0.65])

Figure 2-4. Quarkus Welcome Page

# TIP

You can stop the app running in dev mode by using CTRL-C from the same terminal where you launched it.

You can now try querying one of the items we inserted from the import.sql file, to test if our microservice is running properly.

Just navigate to http://localhost:8080/api/inventory/329299