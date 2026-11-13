
```
routes.get("", use: addresses)
routes.post("", use: self.create)
routes.patch(":id", use: self.update)
routes.delete(":id", use: delete)
```

Just as before, the base route (`/*/users/addresses`) is already given to this function, so we don't have to write all this down. You should be very familiar with the first two lines and understand what they do.

The third and fourth lines are adding a new element: `:id`.

When you write down the paths for a route, you have done this so far by just writing it as a string, such as `users`. Technically, you can specify a path in a few different ways, though. One way is to supply a `PathComponent` of a `RouteComponent`. That could look like this:

```
router.get("a/b/:id", use: func) // "/a/b/:id"
```

Any class that conforms to a `Parameter` can be used as a `RouteComponent`. In our case, we simply want to pass on the ID of the address, so the parameter is of the type `Int`.

You might wonder how it is possible that both routes result in the same URL. Remember that it is always tied to the `HTTP` method. We are registering the routes for `PATCH` as well as `DELETE`. The URL itself would not be able to be called without specifying the `HTTP` method. However, we often tend to assume that it is a `GET` call, if not specified otherwise.

Okay—now, let's fill out the functions.

## Getting addresses

Returning all addresses for a user is simple. You might have guessed it already, but we have some nice helper functions that make this easy. The whole body of this function is only one line.

Write down the following line:

```
return Address.query(on: request.db).filter(\.$userId == request.payload.id).all().map { addresses in
    return addresses.map { AddressResponse($0) }
}
```

Since we are returning an `EventLoopFuture<[AddressResponse]>`, we do have to modify the result from Fluent. Note that this function is only called after the user has been verified, so we can assume that the payload ID is authentic.

Next, let's work on the `create` function.

## Creating addresses

To create an address, we will have to get the authorized user first, then build the new address, and then save it.

Go through the following steps:

1. Write the following line into the `create` function:

```
let content = try request.content.decode(AddressInput.self)
```

This should be very familiar to you by now; we are simply parsing the incoming content.

2. Next, build the address, like this:

```
let address = Address(street: content.street, city: content.city,
    zip: content.zip, userId: request.payload.id)
```

Remember that Vapor already ensured that the user content passed on is conforming to `AddressInput`.

While Vapor is making sure that the content of our input is matching our struct, Vapor does not know specific rules. If you wanted to make sure that a ZIP code is at least five digits long, you would need to enter that in the controller.

3. Finally, we need to save the address and return the `UserSuccessResponse`, as follows:

```
return address.save(on: request.db).map { _ in
    return AddressResponse(address)
}
```

And that's it! Easy, right? Now, let's move on to the next function: `Update Address`.

## Updating addresses

Updating the address is fairly similar to how we updated the user. The main difference is that we are saving a model that is *connected* to the user.

Go through the following steps:

1. Start by writing down our standard line, like this:

```
let content = try request.content.decode(AddressInput.self)
```

2. Now, grab the ID from the router, like this:

```
let id = try request.query.get(Int.self)
```

This ID is of type `Int` (not optional), and it has to be present. If the requester tries to send something other than `Int`, the `Router` will reject the call.

Next, we need to find the correct address in the database. We know the ID of the address, and we know the user ID of the user who owns this address. So, we will simply make both conditions for the database query that Fluent is generating.

3. Write down the following `return` function:

```
return Address.query(on: request.db).filter(\.$id == id).filter(\.$userId == request.payload.id).all().flatMap { addresses in
    
}
```

The first part should look familiar: We are building a `QueryBuilder` address with Fluent. Next, we filter out the ones that match our ID, and then, we also filter out the ones that match the user ID.

You might be wondering why we want to filter out the ones that match our user ID, and not just compare the user ID later on with the address that we find. You really could do both, and it would be fine. However, by adding the user ID as a filter to the query, we are utilizing the database server's CPU and RAM to filter out, and not our own. This also means we don't have to output another error message and make sure it works. We can simply assume that we only get the results we want.

4. Add the following code to the `callback` function after the `addresses in` line:

```
if addresses.count == 0 {
    return request.eventLoop.makeFailedFuture(
        Abort(.badRequest, reason: "No address found!"))
}
let address = addresses.first!
```

While we can offload the user ID verification, Fluent might not find any rows and return an empty array. In this case, we do want to abort and return an error. Note that this also happens if the user ID does not match; we are not differentiating the cause.

5. Next, add the following lines after the `let address ...` statement:

```
address.street = content.street
address.city = content.city
address.zip = content.zip
```

This is simply updating the resulting address. We know, based on our database layout, that we will only have one result in the array if we get to this point. So, we grab it and store it in `address`. Then, we assign new values from the `AddressInput` struct.

6. Finally, we need to save the address and return the `AddressResponse`, as follows:

```
return address.save(on: request.db).map { _ in
    return AddressResponse(address)
}
```

We are done with the `update` function. Now, let's finish the service with the `delete` function.

## Deleting addresses

The `delete` function matches the `update` function until we save or delete the address.

Go through the following steps:

1. Fill out the following for the `delete` function:

```
let id = try request.query.get(Int?.self)

return Address.query(on: request.db).filter(
    \.$id == id!).filter(\.$userId == request.
    payload.id).all().flatMap { addresses in
    if addresses.count == 0 {
        return request.eventLoop.makeFailedFuture(
            Abort(.badRequest, reason: "No address found!"))
    }
    
}
```

All of this is identical to the `update` function.

2. Now, add a return call, right before the last `}`, like this:

```
return addresses.first!.delete(on: request.db).transform(to: .ok)
```

We are grabbing the first address of the array—the only address that we have in there. Then, we tell Fluent to delete it and return a simple `OK` `HTTPStatus`.

And that's it! We are done writing the service—yay! So, let's earn the fruits of our hard work, and start the service.

## Starting the service

You've made it! We now need to compile the service and start it. To do that, run the following commands:

1. Run `swift build`.
2. Run `./.build/debug/Run`.

If you run into any problems after running `swift build`, you should check the following:

* Did you make any little spelling mistakes? As you know, Swift doesn't forgive those.
* Is Swift able to pull all the correct dependencies?

Otherwise, if you run the second command and the service quits very quickly, be sure to check the following:

* The credentials are correctly in the environment. See in the preceding sections for how to do that.
