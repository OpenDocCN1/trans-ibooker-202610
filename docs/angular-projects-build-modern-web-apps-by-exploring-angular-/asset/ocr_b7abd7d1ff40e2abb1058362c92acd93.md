```
output-path=false --watch\" \"webpack --config
webpack.dev.config.js --watch\"",
"build:electron": "ng build && webpack --config
webpack.prod.config.js"
}
```

The build:electron script builds the Angular and Electron application in production mode simultaneously.

We have completed all the configurations needed for packaging our desktop application. In the following section, we will learn how to convert it into a single bundle specific to each operating system.

# Using an Electron bundler

The Electron framework has a wide variety of tools that are created and maintained by the open source community.

You can see a list of available projects in the Tools section at the following link:

https://www.electronjs.org/community

One of these tools is the electron-packager library, which we can use to package our desktop application as a single executable file for each OS (Windows, Linux, and macOS). Let’s see how we can integrate it into our development workflow:

+   1. Run the following npm command to install electron-packager as a development dependency to our project:

```
npm install -D electron-packager
```

+   2. Add a new entry in the scripts property of the package.json file for packaging our application:

```
"scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "watch": "ng build --watch --configuration
    development",
    "test": "ng test",
    "start:desktop": "concurrently \"ng build --
```