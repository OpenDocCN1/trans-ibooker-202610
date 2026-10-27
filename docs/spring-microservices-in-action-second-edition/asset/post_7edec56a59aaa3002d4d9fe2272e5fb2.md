
## 9.3.1 Adding Keycloak to Docker

This section explains how to add the Keycloak service to our Docker environment. To achieve this, let’s start by adding the code shown in the following listing to our docker-compose.yml file.

> **NOTE** In case you didn’t follow the previous chapter’s code listings, you can download the code created in chapter 8 from the following link: https://github.com/ihuaylupo/manning-smia/tree/master/chapter8.

**Listing 9.1** Adding a Keycloak service in docker-compose.yml

```
//Part of docker-compose.yml omitted for conciseness
...
keycloak:
  image: jboss/keycloak
  restart: always
  environment:
    KEYCLOAK_USER: admin
    KEYCLOAK_PASSWORD: admin
  ports:
    - "8080:8080"
  networks:
    backend:
      aliases:
        - "keycloak"
```

- Keycloak Docker service name
- Keycloak Admin Console’s username
- Keycloak Admin Console’s password

> **NOTE** You can use Keycloak with several databases like H2, PostgreSQL, MySQL, Microsoft SQL Server, Oracle, and MariaDB. For the examples in this chapter, we’ll use the default embedded H2 database. We highly recommend visiting the following link if you want to use another database: https://github.com/keycloak/keycloak-containers/tree/master/docker-compose-examples.

Notice that in the code in listing 9.1, we used port 8080 for Keycloak. If we go back a few chapters, we also exposed the licensing service in that port. We will map the licensing service in the docker-compose.yml file to port 8180 instead of 8080 to make everything work, but in the meantime, the following code shows you how to change that port number:

```
licensingservice:
  image: ostock/licensing-service:0.0.3-SNAPSHOT
  ports:
    - "8180:8080"
```

> **NOTE** To make Keycloak work in our local environment, we need to add the host entry 127.0.0.1 Keycloak to our hosts file. If you are using Windows, you need to add the host entry at C:\Windows\System32\drivers\etc\hosts, and if you are using Linux, the host file is at /etc/hosts. Why do we need that entry? The containers can talk to each other using the network aliases or MAC addresses, but our Postman needs to use localhost to invoke the services.

## 9.3.2 Setting up Keycloak

Now that we have Keycloak in the docker-compose.yml, let’s run the following command. We’ll do this at the root folder of our code:

```
docker-compose -f docker/docker-compose.yml up
```

Once the services are up, let’s visit the following link to open the Keycloak Administration Console: [http://keycloak:8080/auth/](http://keycloak:8080/auth/). Configuring Keycloak is a straightforward process. The first time we access Keycloak, a Welcome page is displayed. This page shows different options, such as visiting the Administration Console, documentation, reporting issues, and more. In our case, we want to select Administration Console. Figure 9.2 shows the Welcome page.

![](img/3c6dc613426280ca3eafe43dea24391e_275_0.png)

**Figure 9.2** The Keycloak Welcome page

The next step is to enter the username and password that we previously defined in the docker-compose.yml file. Figure 9.3 shows this step.

![](img/3c6dc613426280ca3eafe43dea24391e_276_0.png)

Figure 9.4 shows how to create the spmia-realm in our Keycloak service.

![](img/3c6dc613426280ca3eafe43dea24391e_276_1.png)

Once the realm is created, you will see the main page for spmia-realm with the configuration shown in figure 9.5.

![](img/3c6dc613426280ca3eafe43dea24391e_277_0.png)

**Figure 9.5** Our Keycloak Spmia-realm configuration page

## 9.3.3 Registering a client application

The next step in our configuration is to create a client. Clients in Keycloak are entities that can request user authentication. The clients are often the applications or services that we want to secure by providing a single sign-on (SSO) solution. To create a client, let’s click the Clients option on the left menu. Once clicked, you will see the page shown in figure 9.6.

![](img/3c6dc613426280ca3eafe43dea24391e_277_1.png)

**Figure 9.6** O-stock’s Spmia-realm Clients page

Once the Clients list is shown, click the Create button (displayed in the table’s top-right corner as shown in the previous figure). Once clicked, you will see an Add Client form that asks for the following information:

- Client ID
- Client Protocol
- Root URL

Enter the information as shown in figure 9.7.

![](img/3c6dc613426280ca3eafe43dea24391e_278_0.png)

**Figure 9.7** O-stock's Keycloak client information

After saving the client, you will be presented with the Client Configuration page shown in figure 9.8. On that page, we’ll enter the following data:

- Access Type: Confidential
- Service Accounts Enabled: On
- Authorization Enabled: On
- Valid Redirect URLs: http://localhost:80*
- Web Origins: *

For this example, we only created a global client called ostock, but here’s where you can configure as many clients as you want.

The next step is to set up the client roles, so let’s click the Roles tab. To better understand client roles, let’s imagine that our application will have two types of users: admins and regular users. The admin users can execute all of the application services, and the regular users are only going to be allowed to execute some of the services.

Once the Roles page is loaded, you will see a list of predefined client roles. Let’s click the Add Role button displayed in the top-right corner of the Roles table. Once clicked, you will see the Add Role form shown in figure 9.9.

![](img/3c6dc613426280ca3eafe43dea24391e_279_0.png)

**Figure 9.8** Additional configuration for O-stock’s Keycloak client

![](img/3c6dc613426280ca3eafe43dea24391e_279_1.png)

**Figure 9.9** O-stock’s Keycloak Add Role page

On the Add Role page, we need to create the following client roles. In the end, you will have a list similar to the one shown in figure 9.10.

- USER
- ADMIN

![](img/3c6dc613426280ca3eafe43dea24391e_280_0.png)

**Figure 9.10** O-stock's Keycloak client roles: USER and ADMIN

Now that we have finished our basic client configuration, let’s visit the Credentials page. The Credentials page will show the required client secret for the authentication process. Figure 9.11 shows how the Credentials page looks.

![](img/3c6dc613426280ca3eafe43dea24391e_280_1.png)

**Figure 9.11** O-stock’s Keycloak client secret on the Credentials page

The next step in our configuration is to create realm roles. The realm roles will allow us to have better control of what roles are being set for each user. This is an optional step. If you don’t want to create these roles, you can go ahead and create the users directly. But later on, it might be harder to identify and maintain the roles for each user.

To create the realm roles, let’s click the Roles option shown in the left menu and then click the Add Role button at the table’s top right. As with the client roles, we will create two types of realm roles: the ostock-user and the ostock-admin. Figures 9.12 and 9.13 show you how to create the ostock-admin realm role.

![](img/3c6dc613426280ca3eafe43dea24391e_281_0.png)

**Figure 9.12** Creating the ostock-admin realm role

![](img/3c6dc613426280ca3eafe43dea24391e_281_1.png)

**Figure 9.13** Specifying additional configurations for the ostock-admin realm role

Now that we have the ostock-admin realm role configured, let’s repeat the same steps to create the ostock-user role. Once you are done, you should have a list similar to the one shown in figure 9.14.

### Roles

![](img/3c6dc613426280ca3eafe43dea24391e_282_0.png)

**Figure 9.14** A list of O-stock’s spmia-realm roles

## 9.3.4 Configuring O-stock users

Now that we’ve defined application-level and realm-level roles, names, and secrets, we’re ready to set up individual user credentials and the roles for the users. To create the users, let’s click the Users option shown in the left menu in Keycloak’s admin console.

For the examples in this chapter, we will define two user accounts: illary.huaylupo and john.carnell. The john.carnell account will have the role of ostock-user and the illary.huaylupo account will have the role of ostock-admin. Figure 9.15 shows the Add User page. On this page, let’s type the username and enable the user and the email verified options as shown in the figure.

![](img/3c6dc613426280ca3eafe43dea24391e_282_1.png)

**Figure 9.15** Keycloak’s Add User page for O-stock’s spmia-realm
