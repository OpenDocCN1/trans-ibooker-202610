As previously mentioned, the Keycloak security components interact together to authenticate the service users. Users authenticate with the Keycloak server by providing their credentials and the application/device they’re using to access a protected resource (the microservice). If the users’ credentials are valid, the Keycloak server provides an authentication token that can be passed from service to service each time a service is used by the users.

The protected resource can then contact the Keycloak server to determine the token’s validity and to retrieve assigned roles for a user. Roles are used to group related users together and to define which resources they can access. For this chapter, we’ll use Keycloak roles to determine what HTTP verbs a user can use to call on which authorized service endpoints.

Web service security is an extremely complicated subject. We need to understand who’s going to call our services (internal users or external users), how they’re going to call our service (internal web-based client, mobile device, or web application), and what actions they’re going to take with our code.

# On authentication vs. authorization

We’ve often found that developers “mix and match” the meaning of the terms authentication and authorization. Authentication is the act of a user proving who they are by providing credentials. Authorization determines whether a user is allowed to do what they want to do. For instance, user Illary could prove her identity by providing a user ID and password, but she may not be authorized to look at sensitive data (payroll data, for example). For our discussion, a user must be authenticated before authorization takes place.

# 9.3 Starting small: Using Spring and Keycloak to protect a single endpoint

To understand how to set up the authentication and authorization pieces, we’ll do the following:

+   - Add a Keycloak service to Docker.
+   - Set up a Keycloak service and register the O-stock application as an authorized application that can authenticate and authorize user identities.
+   - Use Spring Security to protect our O-stock services. We’re not going to build a UI for O-stock; instead, we’ll simulate a user logging in to Postman to provide authentication for our Keycloak service.
+   - Protect the licensing and organization services so that they can only be called by an authenticated user.