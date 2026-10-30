You will see that executing each method produces the same JSON results.

# The ApiController Attribute

The ApiController attribute, added in ASP.NET Core 2.1, provides REST-specific rules, conventions, and behaviors when combined with the ControllerBase class. These conventions and behaviors are outlined in the following sections.

# Attribute Routing Requirement

When using the ApiController attribute, the controller must use attribute routing. This is just enforcing what many consider to be a best practice.

# Automatic 400 Responses

If there is an issue with model binding, the action will automatically return an HTTP 400 (Bad Request) response code. This replaces the following code:

```
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

ASP.NET Core uses the ModelStateInvalidFilter action filter to do the preceding check. When there is a binding or a validation error, the HTTP 400 response in the body includes details for the errors. An example is shown here:

```
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

This behavior can be disabled through configuration in the ConfigureServices() method of the Startup.cs class.

```
services.AddControllers()
    .ConfigureApiBehaviorOptions(options =>
    {
        options.SuppressModelStateInvalidFilter = true;
    });
```