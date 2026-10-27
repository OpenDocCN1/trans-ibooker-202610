```
```java
.anyRequest().authenticated();
http.csrf().disable();
}

@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth)
        throws Exception {
    KeycloakAuthenticationProvider keycloakAuthenticationProvider =
            keycloakAuthenticationProvider();
    keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(new
            SimpleAuthorityMapper());
    auth.authenticationProvider(keycloakAuthenticationProvider);
}

@Bean
@Override
protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
    return new RegisterSessionAuthenticationStrategy(
            new SessionRegistryImpl());
}

@Bean
public KeycloakConfigResolver KeycloakConfigResolver() {
    return new KeycloakSpringBootConfigResolver();
}
}
```
```

The final step in configuring the licensing service is to add the Keycloak configuration to our licensing-service.properties file. The following listing shows how this file should look.

# Listing 9.9   Configuring Keycloak in licensing-service.properties

```
```properties
// Some properties omitted for conciseness

keycloak.realm = spmia-realm
keycloak.auth-server-url = http://keycloak:8080/auth
keycloak.ssl-required = external
keycloak.resource = ostock
keycloak.credentials.secret = 5988f899-a5bf-4f76-b15f-f1cd0d2c81ba
keycloak.use-resource-role-mappings = true
keycloak.bearer-only = true
```
```

Now that we have the gateway changes to propagate the authorization header and the licensing service set up, we can move on to our final step, which is to propagate the access token. All we need to do for this step is to modify how the code in the licensing service calls the organization service. For that, we need to ensure that the HTTP Authorization header is injected into the application call to the organization service.

Without Spring Security, we’d have to write a servlet filter to grab the HTTP header of the incoming licensing service call and then manually add it to every outbound service call in the licensing service. Keycloak provides a new REST template class that supports these calls. The class is called KeycloakRestTemplate. To use this