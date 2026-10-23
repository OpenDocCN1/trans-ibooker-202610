# Forms Authentication

Forms Authentication is implemented into an ASP.NET application starting with the installation of the IIS module. Then, its configuration is installed into the web.config file using syntax similar to the following snippet:

```
<configuration>
    <system.web>
        <authentication mode="Forms" />
    </system.web>
</configuration>
```

Figure 7.3 shows that the IIS module for Forms Authentication is installed but not yet enabled.

# FIGURE 7.3 IIS authentication providers

![](bbox=[0.195, 0.39, 0.79, 0.585])

Enabling the IIS module and the web.config file results in the authentication provider being enabled. The scope of this configuration is for the website running within and underneath the directory structure in direct relation to the location of the physical web.config file.

The reason for the note about where the configuration is applied in relation to the location of the web.config file is that this authentication provider can be enabled on the web server, but it’s not used unless you enabled it specifically on a website. In addition to configuring web.config, you must also have an Authentication (AuthN), in which your custom user identity database stores the credentials. Any group associations that define whether the user has access to specific resources within the application once authenticated, would utilize Authorization (AuthZ) to confirm the access. The code required to perform the AuthN and AuthZ procedure must be written by a developer working in the application itself. This is not an “out-of-the-box” authentication solution. It is, however, a solid, sophisticated, and valid approach for implementing security into an internet application.