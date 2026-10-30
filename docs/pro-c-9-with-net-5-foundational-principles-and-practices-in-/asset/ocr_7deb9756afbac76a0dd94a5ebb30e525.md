```
}
},
"AllowedHosts": "*"
}
```

The template also creates an appsettings.Development.json file. The configuration system works in conjunction with the runtime environment awareness to load additional configuration files based on the runtime environment. This is accomplished by instructing the configuration system to load a file named appsettings.{environmentname}.json after the appSettings.json file. When running under Development, the appsettings.Development.json file is loaded after the initial settings file. If the environment is Staging, the appsettings.Staging.json file is loaded. It is important to note that when more than one file is loaded, any settings that appear in both files are overwritten by the last file loaded; they are not additive.

All configuration values are accessible through an instance of IConfiguration, available through the ASP.NET Core dependency injection system.

# Retrieving Settings

Once the configuration is built, settings can be accessed using the traditional Get family of methods, such as GetSection(), GetValue(), and so on.

```
Configuration.GetSection("Logging")
```

There is also a shortcut for getting application connection strings.

```
Configuration.GetConnectionString("AutoLot")
```

Additional configuration features will be utilized throughout the rest of this book.

# Deploying ASP.NET Core Applications

Prior versions of ASP.NET applications could only be deployed to Windows servers using IIS. ASP.NET Core can be deployed to multiple operating systems in multiple ways, including outside of a web server. The high-level options are as follows:

+   - On a Windows server (including Azure) using IIS
+   - On a Windows server (including Azure app services) outside of IIS
+   - On a Linux server using Apache or NGINX
+   - On Windows or Linux in a Docker container

This flexibility allows organizations to decide the deployment platform that makes the most sense for the organization, including popular container-based deployment models (such as using Docker), as opposed to being locked into Windows servers.

# Lightweight and Modular HTTP Request Pipeline

Following along with the principles of .NET Core, you must opt in for everything in ASP.NET Core. By default, nothing is loaded into an application. This enables applications to be as lightweight as possible, improving performance and minimizing the surface area and potential risk.