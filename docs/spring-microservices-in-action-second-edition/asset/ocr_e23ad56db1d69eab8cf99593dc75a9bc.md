Now we’ll simulate a user that wants to acquire an access token. We’ll do this by using Postman to POST to the endpoint `http://keycloak:8080/auth/realms/spmia-realm/protocol/openid-connect/token` and then provide the application, secret key, user ID, and password.

> **NOTE** Remember, in this example, we use the 8080 port because it is the port we previously defined in the docker-compose.yml file for Keycloak.

To simulate a user acquiring an authentication token, we need to set up Postman with the application name and secret key. For this, we’ll pass these elements to our authentication server endpoint using basic authentication. Figure 9.20 shows how Postman is set up to execute a basic authentication call. Note that we will use the application name we previously defined and the secret application key as a password:

```
Username: <CLIENT_APPLICATION_NAME>
Password: <CLIENT_APPLICATION_SECRET>
```

![](bbox=[0.17, 0.385, 0.875, 0.623])

However, we’re not ready to make the call to get the token just yet. Once the application name and secret key are configured, we need to pass in the following information to the service as HTTP form parameters:

+   - `grant_type`—The grant type to execute. In this example, we’ll use a password grant.
+   - `username`—Name of the user logging in.
+   - `password`—Password of the user logging in.

Figure 9.21 shows how these HTTP form parameters are configured for our authentication call.

Unlike other REST calls in this book, this list’s parameters will not be passed in as a JSON body. The authentication standard expects all parameters passed to the token