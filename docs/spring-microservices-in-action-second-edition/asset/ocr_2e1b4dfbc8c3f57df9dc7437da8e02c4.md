```
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
```

Maps the HTTP request body to a License object

Post method to insert a license

Delete method to delete a license

In listing 3.5, we first annotate the updateLicense() method with a method-level @PutMapping annotation. This annotation acts as a shortcut for the @RequestMapping (method = RequestMethod.PUT) annotation, which we haven’t used yet.

The next thing to note is that we use the @PathVariable and the @RequestBody annotations in the parameter body of the updateLicense() method. @RequestBody maps the HTTPRequest body to a transfer object (in this case, the License object). In the updateLicense() method, we map to the createLicense method from two parameters (one from the URL and the other from the HTTPRequest body) to the following two parameter-level variables:

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