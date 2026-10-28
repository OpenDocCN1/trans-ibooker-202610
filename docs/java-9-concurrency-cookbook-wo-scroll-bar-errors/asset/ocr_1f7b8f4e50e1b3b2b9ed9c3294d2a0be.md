# Taking precautions using lazy initialization

**Lazy initialization** is a common programming technique that delays object creation until it is needed for the first time. This normally causes the initialization of the objects to be made in the implementation of the operations, instead of the constructor of the classes. The main advantage of this technique is that you can save memory. This is because you only create the indispensable objects needed for the execution of your applications. You could have declared a lot of objects in one class, but you don't use every object in every execution of your program; therefore, your application doesn't use the memory needed for the objects that you don't use in an execution of the program. This advantage can be very useful for applications that run in environments with limited resources.

By contrast, this technique has the disadvantage of having performance issues in your application, as you create objects the first time they are used inside an operation.

This technique can also provoke problems if you use it in concurrent applications. As more than one thread can be executing an operation at a time, they can be creating an object at the same time, and this situation can be problematic. This has a special importance with **singleton** classes. An application has only one object of these classes and, as mentioned earlier, a concurrent application can create more than one object. Consider the following code:

```
public static DBConnection getConnection() {
    if (connection==null) {
        connection=new DBConnection();
    }
    return connection;
}
```

This is the typical method in a singleton class to obtain the reference of the unique object of that class existing in the application, using lazy initialization. If the object hasn't been created yet, it creates the object. Finally, it always returns it.

If two or more threads executes at the same time the comparison of the first sentence (`connection == null`), all of them will create a Connection object. This isn't a desirable situation.

In this recipe, you will implement an elegant solution to the lazy initialization problem.