```
```java
return responseMessage;
}

public String updateLicense(License license, String organizationId){
    String responseMessage = null;
    if (license != null) {
        license.setOrganizationId(organizationId);
        responseMessage = String.format(messages.getMessage(
                "license.update.message", null, null),
                license.toString());
    }

    return responseMessage;
}
```
```

![](bbox=[0.649, 0.23, 0.698, 0.266])

There are three important things to highlight from the code in listing 3.9. The first is that we can receive the Locale from the Controller itself. The second is that we can call the messages.getMessage("license.create.message", null, locale) using the locale we received by parameters, and the third thing to note is that we can call the messages.getMessage("license.update.message", null, null) without sending any locale. In this particular scenario, the application will use the default locale we previously defined in the bootstrap class. Now let’s update our createLicense() method on the controller to receive the language from the request Accept-Language header with this code:

```
```java
@PostMapping
public ResponseEntity<String> createLicense(
    @PathVariable("organizationId") String organizationId,
    @RequestBody License request,
    @RequestHeader(value = "Accept-Language", required = false)
        Locale locale){
    return ResponseEntity.ok(licenseService.createLicense(
            request, organizationId, locale));
}
```
```

A few things to note from this code is that here we use the @RequestHeader annotation. The @RequestHeader annotation maps method parameters with request header values. In the createLicense() method, we retrieve the locale from the request header Accept-Language. This service parameter is not required, so if it’s not specified, we will use the default locale. Figure 3.7 shows you how to send the Accept-Language request header from Postman.

> **NOTE** There isn’t a well-defined rule on how to use locale. Our recommendation is to analyze your architecture and select the option that is most suitable for you. For example, if the front-end application handles the locale, then receiving the locale as a parameter in the controller method is the best option. But if you are managing the locale in the backend, you can use a default locale.