# Navigating changes in magic methods

PHP **magic methods** are predefined hooks that interrupt the normal flow of an OOP application. Each magic method, if defined, alters the behavior of the application from the minute the object instance is created, up until the point where the instance goes **out of scope**.

> **Important note**
> An object instance goes *out of scope* when it's unset or overwritten. Object instances also go out of scope when defined in a function or class method, and the execution of that function or class method ends. Ultimately, if for no other reason, an object instance goes out of scope when the PHP program ends.

This section will give you a solid understanding of important changes to magic method usage and behavior introduced in PHP 8. Once you understand the situations described in this section, you will be in a position to make the appropriate code modifications to prevent your application code from failing should you migrate to PHP 8.

Let's first have a look at changes to the object construct method.

# Dealing with constructor changes

Ideally, the **class constructor** is a method that's called automatically when the object instance is created and is used to perform some sort of object initialization. This initialization most typically involves populating object properties with values supplied as arguments to this method. The initialization could also perform any necessary tasks such as opening file handles, establishing a database connection, and so forth.

In PHP 8, a number of changes in how the class constructor is invoked have been made. This means there's a potential for a backwards compatibility break when you migrate your application to PHP 8. The first change we'll examine has to do with deprecated usage of a method with the same name as the class being used as the class constructor.

# Handling changes in a method and class of the same name

In the first PHP OOP implementation, introduced in PHP version 4, it was determined that a method with the same name as the class would assume the role of class constructor, and would be automatically called when a new object instance was created.

It's a little known fact that, even in PHP 8, functions, methods, and even class names, are *case-insensitive*. Thus `$a = new ArrayObject();` is equivalent to `$b = new arrayobject();`. Variable names, on the other hand, are case-sensitive.