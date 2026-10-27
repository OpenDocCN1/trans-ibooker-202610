
> **NOTE** Keycloak allows us to also add additional attributes to the users, such as first name, last name, email, address, birth date, phone number, and more. But, for this example, we will only set up the required attributes.

Once this form is saved, click the Credentials tab. You’ll need to type the user’s password, disable the Temporary option, and click the Set Password button. Figure 9.16 shows this step.

![](img/3c6dc613426280ca3eafe43dea24391e_283_0.png)

Figure 9.16 Setting the user’s password and disabling the Temporary option for O-stock’s user’s credentials

Once the password is set, let’s click the Role Mappings tab and assign the user a specific role. Figure 9.17 shows this step.

![](img/3c6dc613426280ca3eafe43dea24391e_283_1.png)

Figure 9.17 Mapping O-stock’s realm roles to the created user

To finish our configuration, let’s repeat the same steps for the other user, john.carnell in this example, assigning the ostock-user role to John.

## 9.3.5 Authenticating our O-stock users

At this point, we have enough of our base Keycloak server functionality in place to perform application and user authentication for the password grant flow. Let’s start our authentication service. To do this, click the Realm Settings option in the left menu, and then click the OpenID Endpoint Configuration link to see a list of available endpoints for our realm. These steps are shown in figures 9.18 and 9.19.

![](img/3c6dc613426280ca3eafe43dea24391e_284_0.png)

Figure 9.18 Selecting the OpenID Endpoint Configuration link for O-stock’s Keycloak spmia-realm

```json
{
  "issuer": "http://keycloak:8080/auth/realms/spmia-realm",
  "authorization_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/auth",
  "token_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/token",
  "introspection_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/token/introspect",
  "userinfo_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/userinfo",
  "end_session_endpoint": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/logout",
  "jwks_uri": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/certs",
  "check_session_iframe": "http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/login-status-iframe.html",
  "grant_types_supported": [
    "authorization_code",
    "implicit",
    "refresh_token",
    "password",
    "client_credentials"
  ]
}
```

Figure 9.19 The OpenID endpoint configuration for O-stock’s realm

Now we’ll simulate a user that wants to acquire an access token. We’ll do this by using Postman to POST to the endpoint `http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/token` and then provide the application, secret key, user ID, and password.

> **NOTE** Remember, in this example, we use the 8080 port because it is the port we previously defined in the docker-compose.yml file for Keycloak.

To simulate a user acquiring an authentication token, we need to set up Postman with the application name and secret key. For this, we’ll pass these elements to our authentication server endpoint using basic authentication. Figure 9.20 shows how Postman is set up to execute a basic authentication call. Note that we will use the application name we previously defined and the secret application key as a password:

```
Username: <CLIENT_APPLICATION_NAME>
Password: <CLIENT_APPLICATION_SECRET>
```

![](img/3c6dc613426280ca3eafe43dea24391e_285_0.png)

However, we’re not ready to make the call to get the token just yet. Once the application name and secret key are configured, we need to pass in the following information to the service as HTTP form parameters:

- `grant_type`—The grant type to execute. In this example, we’ll use a password grant.
- `username`—Name of the user logging in.
- `password`—Password of the user logging in.

Figure 9.21 shows how these HTTP form parameters are configured for our authentication call.

Figure 9.21 When requesting an access token, the user's credentials are passed in as HTTP form parameters to the /openid-connect/token endpoint.

Unlike other REST calls in this book, this list’s parameters will not be passed in as a JSON body. The authentication standard expects all parameters passed to the token generation endpoint to be HTTP form parameters. Figure 9.22 shows the JSON payload that’s returned from the /openid-connect/token call.

Figure 9.22 JSON payload returned after a successful client credential validation.

The JSON payload contains five attributes:

- access_token—The access token that will be presented with each service call the user makes to a protected resource.
- token_type—The authorization specification allows us to define multiple token types. The most common token type used is the Bearer Token. (We won’t cover any of the other token types in this chapter.)

## 9.4.2 Configuring the service to point to our Keycloak server

Once we set up the organization service as a protected resource, every time a call is made to the service, the caller must include the authentication HTTP header containing a Bearer access token to the service. Our protected resource then has to call back to the Keycloak server to see if the token is valid. Listing 9.3 shows the required Keycloak configuration. You need to add this configuration to the organization service’s application properties file located in the configuration server repository.

```properties
//Some properties omitted for conciseness

keycloak.realm = spmia-realm
keycloak.auth-server-url = http://localhost:8080/auth
keycloak.ssl-required = external
keycloak.resource = ostock
keycloak.credentials.secret = 5988f899-a5bf-4f76-b15f-f1cd0d2c81ba
keycloak.use-resource-role-mappings = true
keycloak.bearer-only = true
```

> **NOTE** To make the examples more straightforward in this book, we use the class-path repository. You’ll find the configuration file in this file: `/configserver/src/main/resources/config/organization-service.properties`.

## 9.4.3 Defining who and what can access the service

We’re now ready to begin defining the access control rules around the service. To define access control rules, we need to extend a KeycloakWebSecurityConfigurerAdapter class and override the following methods:

- `configure()`
- `configureGlobal()`
- `sessionAuthenticationStrategy()`
- `keycloakConfigResolver()`

The organization service’s SecurityConfig class is located in the `/organizationservice/src/main/java/com/optimagrowth/organization/config/SecurityConfig.java` file. The next listing shows the code for this class.

```java
//Some properties omitted for conciseness

@Configuration

@EnableWebSecurity
@EnableGlobalMethodSecurity(jsr250Enabled = true)
public class SecurityConfig extends
    KeycloakWebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http)
        throws Exception {
        super.configure(http);
        http.authorizeRequests()
            .anyRequest()
            .permitAll();
        http.csrf().disable();
    }

    @Autowired
    public void configureGlobal(
        AuthenticationManagerBuilder auth)
        throws Exception {
        KeycloakAuthenticationProvider keycloakAuthenticationProvider =
            keycloakAuthenticationProvider();
        keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(
            new SimpleAuthorityMapper());
        auth.authenticationProvider(keycloakAuthenticationProvider);
    }

    @Bean
    @Override
    protected SessionAuthenticationStrategy
        sessionAuthenticationStrategy() {
        return new RegisterSessionAuthenticationStrategy(
            new SessionRegistryImpl());
    }

    @Bean
    public KeycloakConfigResolver keycloakConfigResolver() {
        return new KeycloakSpringBootConfigResolver();
    }
}
```

- Extends KeycloakWebSecurityConfigurerAdapter
- Registers the Keycloak authentication provider
- Defines the session authentication strategy
- Defines the KeycloakConfigResolver

By default, the Spring Security Adapter looks for a keycloak.json file.

Access rules can range from coarse-grained (any authenticated user can access the entire service) to fine-grained (only the application with this role, but accessing this URL through a DELETE is allowed). We can’t discuss every permutation of Spring Security’s access control rules, but we can look at several of the more common examples. These examples include protecting a resource so that

- Only authenticated users can access a service URL.
- Only users with a specific role can access a service URL.

### PROTECTING A SERVICE WITH AN AUTHENTICATED USER

The first thing we’re going to do is to protect the organization service so that it can only be accessed by an authenticated user. The following listing shows how you can build this rule into the SecurityConfig.java class.
