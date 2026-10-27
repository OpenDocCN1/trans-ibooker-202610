
```
@Service
public class LicenseService {

    public License getLicense(String licenseId, String organizationId){
        License license = new License();
        license.setId(new Random().nextInt(1000));
        license.setLicenseId(licenseId);
        license.setOrganizationId(organizationId);
        license.setDescription("Software product");
        license.setProductName("Ostock");
        license.setLicenseType("full");

        return license;
    }

    public String createLicense(License license, String organizationId){
        String responseMessage = null;
        if(license != null) {
            license.setOrganizationId(organizationId);
            responseMessage = String.format("This is the post and the object is: %s", license.toString());
        }

        return responseMessage;
    }

    public String updateLicense(License license, String organizationId){
        String responseMessage = null;
        if (license != null) {
            license.setOrganizationId(organizationId);
            responseMessage = String.format("This is the put and the object is: %s", license.toString());
        }

        return responseMessage;
    }

    public String deleteLicense(String licenseId, String organizationId){
        String responseMessage = null;
        responseMessage = String.format("Deleting license with id %s for the organization %s", licenseId, organizationId);
        return responseMessage;
    }
}
```

This service class contains a set of dummy services returning hardcoded data to give you an idea of how the skeleton of a microservice should look. As you continue reading, you’ll continue working on this service, and we’ll delve further into how to structure it. For now, let’s add the first method to the controller. This method implements the GET verb used in a REST call and returns a single License class instance as shown in the next code listing.

syntax to the parameters of your method. For the GET service in listing 3.4, we map two parameters from the URL (organizationId and licenseId) to two parameter-level variables in the method, like so:

```
@PathVariable("organizationId") String organizationId
@PathVariable("licenseId") String licenseId
```

Finally, let’s examine the ResponseEntity return object. The ResponseEntity represents the entire HTTP response, including the status code, the headers, and the body. In the previous listing, it allows us to return the License object as the body and the 200 (OK) status code as the HTTP response of the service.

Now that you understand how to create an endpoint using the HTTP GET verb, let’s continue by adding POST, PUT, and DELETE methods to create, update, and delete License class instances. The following listing shows how to do this.

## Listing 3.5 Exposing individual HTTP endpoints

```
package com.optimagrowth.license.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.optimagrowth.license.model.License;
import com.optimagrowth.license.service.LicenseService;

@RestController
@RequestMapping(value="v1/organization/{organizationId}/license")
public class LicenseController {

    @Autowired
    private LicenseService licenseService;

    @RequestMapping(value="/{licenseId}",method = RequestMethod.GET)
    public ResponseEntity<License> getLicense(
        @PathVariable("organizationId") String organizationId,
        @PathVariable("licenseId") String licenseId) {

        License license = licenseService.getLicense(licenseId, organizationId);
        return ResponseEntity.ok(license);
    }

    @PutMapping
    public ResponseEntity<String> updateLicense(
        @PathVariable("organizationId")
        String organizationId,
        @RequestBody License request) {
        return ResponseEntity.ok(licenseService.updateLicense(request, organizationId));
    }

    @PostMapping
    public ResponseEntity<String> createLicense(
        @PathVariable("organizationId") String organizationId,
        @RequestBody License request) {
        return ResponseEntity.ok(licenseService.createLicense(request, organizationId));
    }

    @DeleteMapping(value="/{licenseId}")
    public ResponseEntity<String> deleteLicense(
        @PathVariable("organizationId") String organizationId,
        @PathVariable("licenseId") String licenseId) {
        return ResponseEntity.ok(licenseService.deleteLicense(licenseId, organizationId));
    }
}
```

- PUT method to update a license
- Maps the HTTP request body to a License object
- POST method to insert a license
- DELETE method to delete a license

In listing 3.5, we first annotate the updateLicense() method with a method-level @PutMapping annotation. This annotation acts as a shortcut for the @RequestMapping (method = RequestMethod.PUT) annotation, which we haven’t used yet.

The next thing to note is that we use the @PathVariable and the @RequestBody annotations in the parameter body of the updateLicense() method. @RequestBody maps the HTTP request body to a transfer object (in this case, the License object). In the updateLicense() method, we map to the createLicense method from two parameters (one from the URL and the other from the HTTP request body) to the following two parameter-level variables:

```
@PathVariable("organizationId") String organizationId
@RequestBody License request
```

And finally, in listing 3.5 note that we use the @PostMapping and the @DeleteMapping annotations. The @PostMapping annotation is a method-level annotation that acts as a shortcut for

```
@RequestMapping(method = RequestMethod.POST)
```

@DeleteMapping(value="/{licenseId}") is also a method-level annotation and acts as a shortcut for

```
@RequestMapping(value="/{licenseId}",method = RequestMethod.DELETE)
```

## Endpoint names matter

Before you get too far down the path of writing microservices, make sure that you (and potentially other teams in your organization) establish standards for the endpoints you want to expose via your services. The URLs (Uniform Resource Locator) for the microservice should be used to clearly communicate the intent of the service, the resources the service manages, and the relationships that exist between the resources managed within the service. We’ve found the following guidelines useful for naming service endpoints:

- *Use clear URL names* that establish what resource the service represents. Having a canonical format for defining URLs will help your API feel more intuitive and easier to use—and be consistent in your naming conventions.
- *Use the URL to establish relationships between resources.* Often times, you’ll have a parent-child relationship between resources within your microservices where the child doesn’t exist outside the context of the parent. Hence, you might not have a separate microservice for the child. Use the URLs to express these relationships. If you find that your URLs tend to be long and nested, your microservice may be doing too much.
- *Establish a versioning scheme for URLs early.* The URL and its corresponding endpoints represent a contract between the service owner and the consumer of the service. One common pattern is to prepend all endpoints with a version number. Establish your versioning scheme early and stick to it. It’s extremely difficult to retrofit versioning to URLs (for example, using `/v1/` in the URL mappings) after you already have several consumers using them.

At this point, you have several services. From a command-line window, go to your project root directory, where you’ll find pom.xml, and execute the following Maven command. (Figure 3.4 shows the expected output of this command.)

```
mvn spring-boot:run
```

![](img/3c6dc613426280ca3eafe43dea24391e_101_0.png)

The license server starting on port 8080

Figure 3.4  Output showing that the licensing service successfully starts
