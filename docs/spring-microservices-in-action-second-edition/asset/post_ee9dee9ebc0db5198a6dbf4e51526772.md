
# Listing 9.7 Configuring Keycloak and Spring Security dependencies

```
//Part of pom.xml omitted for conciseness
    <dependency>
        <groupId>org.keycloak</groupId>
        <artifactId>keycloak-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.keycloak.bom</groupId>
            <artifactId>keycloak-adapter-bom</artifactId>
            <version>11.0.2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

The next step is to protect the licensing service so that it can only be accessed by an authenticated user. The following listing shows the SecurityConfig class. You'll find the source code for this class in /licensing-service/src/main/java/com/optimagrowth/license/config/SecurityConfig.java.

# Listing 9.8 Restricting access to authenticated users only

```
//Part of class declaration omitted for conciseness

@Configuration
@EnableWebSecurity
public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http.authorizeRequests()
            .anyRequest().authenticated();
        http.csrf().disable();
    }
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http.authorizeRequests()
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

The final step in configuring the licensing service is to add the Keycloak configuration to our licensing-service.properties file. The following listing shows how this file should look.

# Listing 9.9 Configuring Keycloak in licensing-service.properties

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

Now that we have the gateway changes to propagate the authorization header and the licensing service set up, we can move on to our final step, which is to propagate the access token. All we need to do for this step is to modify how the code in the licensing service calls the organization service. For that, we need to ensure that the HTTP Authorization header is injected into the application call to the organization service.

Without Spring Security, we’d have to write a servlet filter to grab the HTTP header of the incoming licensing service call and then manually add it to every outbound service call in the licensing service. Keycloak provides a new REST template class that supports these calls. The class is called KeycloakRestTemplate. To use this

```
```xml
<dependency>
    <groupId>commons-codec</groupId>
    <artifactId>commons-codec</artifactId>
</dependency>
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>20190722</version>
</dependency>
```
```

Once the libraries are added, we can then add a new method called getUsername() to the tracking filter. The following listing shows this new method. You’ll find the source code for this method in the class file at /gatewayserver/src/main/java/com/optimagrowth/gateway/filters/TrackingFilter.java.

# Listing 9.12 Parsing the preferred username from our JWT

```java
//Part of this code omitted for conciseness
private String getUsername(HttpHeaders requestHeaders){
String username = "";
if (filterUtils.getAuthToken(requestHeaders)!=null){
        String authToken =
                filterUtils.getAuthToken(requestHeaders)
                .replace("Bearer ", "");
        JSONObject jsonObj = decodeJWT(authToken);
        try {
                username =
                        jsonObj.getString("preferred_username");
        }catch(Exception e) {logger.debug(e.getMessage());}
}
return username;
}

private JSONObject decodeJWT(String JWTToken) {
String[] split_string = JWTToken.split("\\.");
String base64EncodedBody = split_string[1];
Base64 base64Url = new Base64(true);
String body = new String(base64Url.decode(base64EncodedBody));
JSONObject jsonObj = new JSONObject(body);
return jsonObj;
}
```

> Parses the token from the authorization HTTP header

> Pulls the preferred_username from the JWT

> Uses Base64 encoding to parse the token, passing in the key that signs the token

> Parses the JWT body into a JSON object to retrieve the preferred_username

To make this example work, we need to make sure the AUTH_TOKEN variable in Filter-Utils is set to Authorization as in the following code snippet. (You’ll find the source for this in /gatewayserver/src/main/java/com/optimagrowth/gateway/filters/Filter-Utils.java.)

```java
public static final String AUTH_TOKEN = "Authorization";
```

Once we implement the getUsername() function, we can add a System.out.println to the filter() method on the tracking filter to print the preferred_username

parsed from our JWT that’s flowing through the gateway. Now when we make a call to the gateway, we’ll see the preferred_username in the console output.

> NOTE When you make this call, you still need to set up all the HTTP form parameters, including the HTTP Authorization header and the JWT.

If everything was successful, you should see the following System.out.println in your console log:

```
tmx-correlation-id found in tracking filter:
26f2b2b7-51f0-4574-9d84-07e563577641.
The authentication name from the token is : illary.huaylupo
```

## 9.5 Some closing thoughts on microservice security

While this chapter has introduced you to OpenID, OAuth2, and the Keycloak specification and how you can use Spring Cloud security with Keycloak to implement an authentication and authorization service, Keycloak is only one piece of the microservice security puzzle. As you shape your microservices for production use, you should also build your microservice security around the following practices:

- Use HTTPS/Secure Sockets Layer (SSL) for all service communications.
- Use an API gateway for all service calls.
- Provide zones for your services (for example, a public API and private API).
- Limit the attack surface of your microservices by locking down unneeded network ports.

Figure 9.28 shows how these different pieces fit together. Each of the bulleted items in the list maps to the numbers in figure 9.28. We’ll examine each of the topic areas enumerated in the previous list and in the figure in more detail throughout the next sections.

![](img/3c6dc613426280ca3eafe43dea24391e_301_0.png)

Figure 9.28 A microservice security architecture is more than implementing authentication and authorization.
