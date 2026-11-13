
Now, we only have to make sure that we have configured our services to work with the correct URLs and we are good to test.

Now that you have learned how to test multiple services together, let's summarize this chapter.

# Summary

In this chapter, you learned how to test microservices. We started by looking at unit tests and what the pros and cons are. Afterward, we looked at functional tests and how they go hand in hand with unit tests. Then, we looked at how to test microservices and how to do that with the help of Postman.

By now, you have learned how to write your own test suites for APIs and verify that everything is working as expected.

You created a test collection for the user service we wrote in [Chapter 7](#de77c71a-d8d1-4d47-9dbe-b621d7690103.xhtml) and verified its functionality. In the next chapter, we will write the next microservice, a product management service, for our example case.

# Questions

Answer the following questions or work through the problems to solidify your understanding. For some of these, you need to work through the external documentation of Postman:

1. What is the fundamental difference between unit tests and functional tests?
2. How can unit tests and functional tests work together?
3. Write Postman tests for all endpoints of the user service.
4. Write more sophisticated tests when verifying the responses (for example, check that the token contains the correct information).
5. Write unit tests verifying the JWT functions in the user service are working correctly.

# Product Management Service

Alright—you have already written your first microservice in [Chapter 7](#de77c71a-d8d1-4d47-9dbe-b621d7690103.xhtml), *Writing the User Service*, and you have learned how to test microservices in [Chapter 8](#cdfd1f9b-2194-40cc-97dd-9c3d49613bfd.xhtml), *Testing Microservices*. Now, we are going to write our next microservice: **Product Management Service (PMS)**!

The PMS will be an important service in our setup, but it will be very different from the **User Management Service (UMS)**. In this chapter, you are going to learn how to write microservices that verify authentication and provide some limited admin management functions. This is the second microservice in our example shop application and will bring us closer to having the application run fully. It will also show you another way microservices can interact with each other since this service is less complex than the UMS. By the end of this chapter, you will have built a second microservice that interacts within the context of the application and serves as the content provider for products.

In this chapter, we will cover the following sections:

* Setting up the project
* Developing routes
* Writing the models we need
* Writing the controllers we need
* Testing the service

Let's get started!

# Technical requirements

This section covers what you will need to have running on your machine so that you can write this microservice. The following components will need to be installed:

* Vapor Toolbox
* Swift 5.2+
* Xcode 11+
* macOS/Linux

We are also assuming that you have worked through the previous chapters and know how to write a Vapor microservice. Going through the creation of this microservice will focus on the *new* elements, not what was covered in [Chapter 7](#de77c71a-d8d1-4d47-9dbe-b621d7690103.xhtml), *Writing the User Service*. All code in this chapter will run on macOS and Linux alike.

The GitHub URL for this chapter is [https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 9/ProductService](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%209/ProductService).

Now, let's start by setting up the project.

# Setting up the project

In this section, you will be instructed on how to set this service up before we start coding the main elements. Setting up this service is a good bit easier than the previous service, the UMS. We are not sending emails, nor are we issuing **JSON Web Tokens (JWTs)**. Remember that JWTs are issued by our UMS, so all we have to do is validate that they are actually issued correctly and have not been tampered with. We will need a database in which to store the product and category information, which—just as before—is going to be MySQL.

What we will do is basically the following:

* Manage models that represent the products and categories.
* Verify that a user is authorized to modify products or categories.

This section covers the following:

* Setting up the template
* Setting up JWT verification
* Setting up the database

Let's start with the template.

## Setting up the template

Let's start setting up the template we created in [Chapter 5](#045da3f4-89ae-4871-bde0-08285e687f02.xhtml), *Creating Your First Microservice*. We don't need to rewrite what we created before, and will simply use it as a basis for this service. Follow these steps:

1. Go into the folder for the product manager, as shown here:

```
$ cd ~/Shop\ Backend/ProductService
```

2. Now, copy the template from the origin folder into our new folder:

```
$ cp -r ../template/. . && rm -rf .build
```

3. Now, open up the `Package.swift` file, like this:

```
$ open Package.swift
```

4. Change the name to `ProductService`.

5. Now, install all dependencies and build the project, as follows:

```
$ swift package update
```

6. Open the project in Xcode, like this:

```
$ xed .
```

Now that we have set up the template, we can start using it. The PMS does not require any additional packages. We do want to verify certain requests to allow admin users to manage the products, so let's go ahead and set up JWT verification.

## Setting up JWT verification

Thanks to our JWT packages we installed with the template, we do not need to do much here. For convenience, let's create a setup file, just as we did in [Chapter 7](#de77c71a-d8d1-4d47-9dbe-b621d7690103.xhtml), *Writing the User Service*. To do so, follow these steps:

1. Create a setup file in the folder, like this:

```
$ open .env
```

2. Open your **JSON Web Key Set (JWKS)** file, which should be saved in your folder above the project folder.

3. Copy the entire JWKS content into one line and paste it into the `.env` file, like this:

```
JWKS='<copy the value from the jwks>'
```

And that's it. We do not need the private key as we are only validating the JWT. Whenever a request comes in and it needs to be verified, the `JWTMiddleware` will automatically check if the JWT payload is correctly signed by using the public key. If you need to familiarize yourself with this, you might want to go back to [Chapter 3](#9b085c0a-3b19-4c38-94b9-05739781ff96.xhtml), *Getting Started with the Vapor Framework*.

Now that we have set up JWT, let's get the database configured.

## Setting up the database

Go ahead and create a new user and a database in your local database setup. If you need a reminder on how to do this, check out [Chapter 3](#9b085c0a-3b19-4c38-94b9-05739781ff96.xhtml), *Getting Started with the Vapor Framework*. Name the user `service_products`, and the database should also be named `service_products`.

To set up the database, all we have to do is the following:

Add the following lines to the `.env` file:

```
MYSQL_CRED='mysql://service_products:password@localhost:3306/service_products?ssl=false'
```

Make sure you have entered the correct password.

We have completed setting everything up now: the template, JWT, and the database. This completes our preparations so that we can now work on the routes.

# Developing routes
