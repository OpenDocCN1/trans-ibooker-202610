
## Controlling XML serialization

In Program.cs, we added the XmlSerializer so that our Web API service can return XML as well as JSON if the client requests that.

However, the XmlSerializer cannot serialize interfaces, and our entity classes use ICollection<T> to define related child entities. This causes a warning at runtime, for example, for the Customer class and its Orders property, as shown in the following output:

```
warn: Microsoft.AspNetCore.Mvc.Formatters.XmlSerializerOutputFormatter[1]
An error occurred while trying to create an XmlSerializer for the type 'Packt.Shared.Customer'.
System.InvalidOperationException: There was an error reflecting type 'Packt.Shared.Customer'.
---> System.InvalidOperationException: Cannot serialize member 'Packt.Shared.Customer.Orders' of type 'System.Collections.Generic.ICollection`1[[Packt.Shared.Order, Northwind.Common.EntityModels, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]', see inner exception for more details.
```

We can prevent this warning by excluding the Orders property when serializing a Customer to XML:

- 1. In the Northwind.Common.EntityModels.Sqlite and the Northwind.Common.EntityModels.SqlServer projects, open Customers.cs.
- 2. Import the System.Xml.Serialization namespace so that we can use the [XmlIgnore] attribute.
- 3. Decorate the Orders property with an attribute to ignore it when serializing, as shown highlighted in the following code:
- 4. In the Northwind.Common.EntityModels.SqlServer project, decorate the CustomerCustomerDemos property with [XmlIgnore] too.

```
[InverseProperty(nameof(Order.Customer))]
[XmlIgnore]
public virtual ICollection<Order> Orders { get; set; }
```

## Documenting and testing web services

You can easily test a web service by making HTTP GET requests using a browser. To test other HTTP methods, we need a more advanced tool.

## Testing GET requests using a browser

You will use Chrome to test the three implementations of a GET request – for all customers, for customers in a specified country, and for a single customer using their unique customer ID:

- 1. Start the Northwind.WebApi web service.
- 2. Start Chrome.
- 3. Navigate to https://localhost:5001/api/customers and note the JSON document returned, containing all 91 customers in the Northwind database (unsorted), as shown in Figure 16.3:
- 4. Navigate to https://localhost:5001/api/customers/?country=Germany and note the JSON document returned, containing only the customers in Germany, as shown in Figure 16.4:
- 5. Navigate to https://localhost:5001/api/customers/alfki and note the JSON document returned containing only the customer named Alfreds Futterkiste, as shown in Figure 16.5:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_0.png)

Figure 16.3: Customers from the Northwind database as a JSON document

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_1.png)

Figure 16.4: A list of customers from Germany as a JSON document

> If you get an empty array returned, then make sure you have entered the country name using the correct casing because the database query is case-sensitive. For example, compare the results of uk and UK.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_2.png)

Figure 16.5: Specific customer information as a JSON document

Unlike with country names, we do not need to worry about casing for the customer id value because inside the controller class, we normalized the string value to uppercase in code.

But how can we test the other HTTP methods, such as POST, PUT, and DELETE? And how can we document our web service so it's easy for anyone to understand how to interact with it?

To solve the first problem, we can install a Visual Studio Code extension named **REST Client**. To solve the second, we can use **Swagger**, the world's most popular technology for documenting and testing HTTP APIs. But first, let's see what is possible with the Visual Studio Code extension.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_720_0.png)

> There are many tools for testing Web APIs, for example, **Postman**. Although Postman is popular, I prefer **REST Client** because it does not hide what is actually happening. I feel Postman is too GUI-y. But I encourage you to explore different tools and find the ones that fit your style. You can learn more about Postman at the following link: https://www.postman.com/

## Testing HTTP requests with the REST Client extension

REST Client is an extension that allows you to send any type of HTTP request and view the response in Visual Studio Code. Even if you prefer to use Visual Studio as your code editor, it is useful to install Visual Studio Code to use an extension like REST Client.

## Making GET requests using REST Client

We will start by creating a file for testing GET requests:

- 1. If you have not already installed REST Client by Huachao Mao (humao.rest-client), then install it in Visual Studio Code now.
- 2. In your preferred code editor, start the Northwind.WebApi project web service.
- 3. In Visual Studio Code, in the PracticalApps folder, create a RestClientTests folder, and then open the folder.
- 4. In the RestClientTests folder, create a file named get-customers.http, and modify its contents to contain an HTTP GET request to retrieve all customers, as shown in the following code:
- 5. In Visual Studio Code, navigate to View | Command Palette, enter rest client, select the command Rest Client: Send Request, and press Enter, as shown in Figure 16.6:

```
GET https://localhost:5001/api/customers/ HTTP/1.1
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_721_0.png)

Figure 16.6: Sending an HTTP GET request using REST Client

6. Note the **Response** is shown in a new tabbed window pane vertically and that you can rearrange the open tabs to a horizontal layout by dragging and dropping tabs.

7. Enter more GET requests, each separated by three hash symbols, to test getting customers in various countries and getting a single customer using their ID, as shown in the following code:

```
###
GET https://localhost:5001/api/customers/?country=Germany HTTP/1.1
###
GET https://localhost:5001/api/customers/?country=USA HTTP/1.1
Accept: application/xml
###
GET https://localhost:5001/api/customers/ALFKI HTTP/1.1
###
GET https://localhost:5001/api/customers/abcxy HTTP/1.1
```

8. Click the **Send Request** link above each request to send it; for example, the GET that has a request header to request customers in the USA as XML instead of JSON, as shown in Figure 16.7:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_721_1.png)

Figure 16.7: Sending a request for XML and getting a response using REST Client

## Making other requests using REST Client

Next, we will create a file for testing other requests like POST:

- 1. In the RestClientTests folder, create a file named create-customer.http and modify its contents to define a POST request to create a new customer, noting that REST Client will provide IntelliSense while you type common HTTP requests, as shown in the following code:

```
POST https://localhost:5001/api/customers/ HTTP/1.1
Content-Type: application/json
Content-Length: 301

{
  "customerID": "ABCXY",
  "companyName": "ABC Corp",
  "contactName": "John Smith",
  "contactTitle": "Sir",
  "address": "Main Street",
  "city": "New York",
  "region": "NY",
  "postalCode": "90210",
  "country": "USA",
  "phone": "(123) 555-1234",
  "fax": null,
  "orders": null
}
```

- 2. Due to different line endings in different operating systems, the value for the Content-Length header will be different on Windows and macOS or Linux. If the value is wrong, then the request will fail. To discover the correct content length, select the body of the request and then look in the status bar for the number of characters, as shown in Figure 16.8:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_722_0.png)

Figure 16.8: Checking the correct content length

3. Send the request and note the response is 201 Created. Also note the location (that is, the URL) of the newly created customer is https://localhost:5001/api/Customers/abcxy, and includes the newly created customer in the response body, as shown in Figure 16.9:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_723_0.png)

Figure 16.9: Adding a new customer

I will leave you an optional challenge to create REST Client files that test updating a customer (using PUT) and deleting a customer (using DELETE). Try them on customers that do exist as well as customers that do not. Solutions are in the GitHub repository for this book.

Now that we've seen a quick and easy way to test our service, which also happens to be a great way to learn HTTP, what about external developers? We want it to be as easy as possible for them to learn and then call our service. For that purpose, we will use Swagger.

## Understanding Swagger

The most important part of Swagger is the OpenAPI Specification, which defines a REST-style contract for your API, detailing all its resources and operations in a human- and machine-readable format for easy development, discovery, and integration.

Developers can use the OpenAPI Specification for a Web API to automatically generate strongly-typed client-side code in their preferred language or library.

For us, another useful feature is Swagger UI, because it automatically generates documentation for your API with built-in visual testing capabilities.

Let's review how Swagger is enabled for our web service using the Swashbuckle package:

- 1. If the web service is running, shut down the web server.
- 2. Open Northwind.WebApi.csproj and note the package reference for Swashbuckle.AspNetCore, as shown in the following markup:

```
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.1.5" />
</ItemGroup>
```
