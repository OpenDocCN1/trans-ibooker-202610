To finish our configuration, let’s repeat the same steps for the other user, john.carnell in this example, assigning the ostock-user role to John.

# 9.3.5 Authenticating our O-stock users

At this point, we have enough of our base Keycloak server functionality in place to perform application and user authentication for the password grant flow. Let’s start our authentication service. To do this, click the Realm Settings option in the left menu, and then click the OpenID Endpoint Configuration link to see a list of available endpoints for our realm. These steps are shown in figures 9.18 and 9.19.

![](bbox=[0.19, 0.27, 0.87, 0.65])

Figure 9.18 Selecting the OpenID Endpoint Configuration link for O-stock’s Keycloak spmia-realm

```
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
  ],
}
```

Figure 9.19 Mapping O-stock’s realm roles to the created user