
Items like your database configuration used by Spring Data won’t be reloaded by this annotation.

## On refreshing microservices

When using Spring Cloud Config service with microservices, one thing you need to consider before you dynamically change properties is that you might have multiple instances of the same service running. You’ll need to refresh all of those services with their new application configurations. There are several ways you can approach this problem.

Spring Cloud Config service offers a push-based mechanism called Spring Cloud Bus that allows the Spring Cloud Configuration Server to publish to all the clients using the service where a change occurs. Spring Cloud Bus requires an extra piece of running middleware: RabbitMQ. This is an extremely useful means of detecting changes, but not all Spring Cloud Config backends support the push mechanism (the Consul server, for example). In the next chapter, you’ll use Spring Cloud service discovery and Eureka to register all instances of a service.

One technique that we’ve used to handle application configuration refresh events is to refresh the application properties in Spring Cloud Config. Then we write a simple script to query the service discovery engine to find all instances of a service and call the `/refresh` endpoint directly.

You can also restart all the servers or containers to pick up the new property. This is a trivial exercise, especially if you’re running your services in a container service such as Docker. Restarting Docker containers literally takes seconds and will force a reread of the application configuration.

Remember that cloud-based servers are ephemeral. Don't be afraid to start new instances of a service with new configurations, then direct traffic to the new services and tear down the old ones.

## 5.3.6 Using Spring Cloud Configuration Server with Git

As mentioned earlier, using a filesystem as the backend repository for the Spring Cloud Configuration Server can be impractical for a cloud-based application. That’s because the development team has to set up and manage a shared filesystem that’s mounted on all instances of the Config Server, and the Config Server integrates with different backend repositories that can be used to host the application configuration properties.

One approach that we’ve used successfully is to employ a Spring Cloud Configuration Server with a Git source control repository. By using Git, you can get all the benefits of putting your configuration management properties under source control and provide an easy mechanism to integrate the deployment of your property configuration files in your build and deployment pipeline. To use Git, we need to add the configuration to the Spring Cloud Config service bootstrap.yml file. The next listing shows how.

### Listing 5.13 Adding Git support to the Spring Cloud bootstrap.yml

```
spring:
  application:
    name: config-server
  profiles:
    active:
      - native, git
  cloud:
    config:
      server:
        native:
          search-locations: classpath:/config
        git:
          uri: https://github.com/ihuaylupo/config.git
          searchPaths: licensingservice
server:
  port: 8071
```

Maps all the profiles (this is a comma-separated list)

Tells Spring Cloud Config to use Git as a backend repository

Tells Spring Cloud Config the URL to the Git server and repo

Tells Spring Cloud Config what path in Git to use to look for the config files

The four key pieces of configuration properties in the previous listing include the following:

- spring.profiles.active
- spring.cloud.config.server.git
- spring.cloud.config.server.git.uri
- spring.cloud.config.server.git.searchPaths

The spring.profiles.active property sets all the active profiles for the Spring Config service. This comma-separated list uses the same precedence rules as a Spring Boot application: active profiles have precedence over default profiles, and the last profile is the winner. The spring.cloud.config.server.git property tells the Spring Cloud Config Server to use a non-filesystem-based backend repository. In the previous listing, we connected to the cloud-based Git repository, GitHub.

> NOTE If you’re authorized to use GitHub, you need to set the username and password (personal token or SSH configuration) in the Git configuration on the bootstrap.yml of the configuration server.

The spring.cloud.config.server.git.uri property provides the URL of the repository you're connecting to. And finally, the spring.cloud.config.server.git.searchPaths property tells the Config Server the relative path on the Git repository that will be searched when the Cloud Config Server boots up. Like the filesystem version of the configuration, the value in the spring.cloud.config.server.git.searchPaths attribute will be a comma-separated list for each service hosted by the configuration service.

> NOTE The default implementation of an environment repository in Spring Cloud Config is the Git backend.

## 5.3.7 Integrating Vault with the Spring Cloud Config service

As mentioned earlier, there is another backend repository that we will use: the HashiCorp Vault. Vault is a tool that allows us to securely access secrets. We can define secrets as any piece of information we want to restrict or control access to, such as passwords, certificates, API keys, and so forth.

To configure Vault in our Spring Config service, we must add a Vault profile. This profile enables integration with Vault and allows us to securely store the application properties of our microservices. To achieve this integration, we will use Docker to create a Vault container with the following command:

```
docker run -d -p 8200:8200 --name vault -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' vault
```

The docker run command contains these parameters:

- VAULT_DEV_ROOT_TOKEN_ID—This parameter sets the ID of the generated root token. The root token is the initial access token to start configuring Vault. This sets the ID of the initial generated root token to a given value.
- VAULT_DEV_LISTEN_ADDRESS—This parameter sets the IP address and port of the development server listener; the default value is 0.0.0.0:8200.

> NOTE In this example, we will run Vault locally. If you need additional info on how to run Vault in server mode, we highly recommend that you visit the official Vault Docker image information at https://hub.docker.com/_/vault.

Once the latest Vault image is pulled into Docker, we can start creating our secrets. To make this example more straightforward, we will use the Vault UI, but if you prefer to move on with the CLI commands, go for it.

## 5.3.8 Vault UI

Vault offers a unified interface that facilitates the process of creating secrets. To access this UI, we need to enter the following URL: http://0.0.0.0:8200/ui/vault/auth. This URL was defined by the VAULT_DEV_LISTEN_ADDRESS parameter set with the docker run command. Figure 5.10 shows the login page for the Vault UI.

The next step is to create a secret. To create the secret after you’ve logged in, click the Secrets tab in the Vault UI dashboard. For this example, we will create a secret called secret/licensingservice with a property called license.vault.property and set its value to Welcome to vault. Remember, access to this piece of information will be restricted and will be encrypted. To achieve this, first we need to create a new secret engine and then add the specific secret to that engine. Figure 5.11 shows how to create this with the Vault UI.

Figure 5.10 Login page in the Vault UI. Enter the following URL in the Token field to sign in: http://0.0.0:8200/ui/vault/auth.

Figure 5.11 Creating a new secret engine in the Vault UI

Now, that we have our new secret engine, let’s create our secret. Figure 5.12 shows you how.

![](img/3c6dc613426280ca3eafe43dea24391e_172_0.png)

Figure 5.12 Creating a new secret in the Vault UI

Now that we have configured the Vault and a secret, let’s configure our Spring Cloud Config Server to communicate with Vault. To do that, we’ll add the Vault profile to our bootstrap.yml file for the Config Server. The next listing shows how your bootstrap file should look.

### Listing 5.14 Adding Vault to the Spring Cloud bootstrap.yml

```
spring:
  application:
    name: config-server
  profiles:
    active:
      - vault
  cloud:
    config:
      server:
        vault:
          port: 8200
          host: 127.0.0.1
          kvVersion: 2
server:
  port: 8071
```

Tells Spring Cloud Config to use Vault as the backend repository
Tells Spring Cloud Config the Vault host
Sets the kv secrets engine version.
Tells Spring Cloud Config the Vault port
