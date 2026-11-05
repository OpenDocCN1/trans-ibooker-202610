```
[HttpDelete("{id}")]
[ProducesResponseType(204)]
[ProducesResponseType(400)]
[ProducesResponseType(404)]
public async Task<IActionResult> Delete(string id)
{
    Customer? existing = await repo.RetrieveAsync(id);
    if (existing == null)
    {
        return NotFound(); // 404 Resource not found
    }

    bool? deleted = await repo.DeleteAsync(id);

    if (deleted.HasValue && deleted.Value) // short circuit AND
    {
        return new NoContentResult(); // 204 No content
    }
    else
    {
        return BadRequest( // 400 Bad request
            $"Customer {id} was found but failed to delete.");
    }
}
```

While reviewing this Web API controller class, note the following:

+   - The Controller class registers a route that starts with api/ and includes the name of the controller, that is, api/customers.
+   - The constructor uses dependency injection to get the registered repository for working with customers.
+   - There are five action methods to perform CRUD operations on customers—two GET methods (for all customers or one customer), POST (create), PUT (update), and DELETE.
+   - The GetCustomers method can have a string parameter passed with a country name. If it is missing, all customers are returned. If it is present, it is used to filter customers by country.
+   - The GetCustomer method has a route explicitly named GetCustomer so that it can be used to generate a URL after inserting a new customer.
+   - The Create and Update methods both decorate the customer parameter with [FromBody] to tell the model binder to populate it with values from the body of the POST request.
+   - The Create method returns a response that uses the GetCustomer route so that the client knows how to get the newly created resource in the future. We are matching up two methods to create and then get a customer.