
The preceding command will build the application and start deploying it to the selected Firebase project. Once deployment is complete, the Angular CLI will report back the following information:

- **Project Console**: The dashboard of the Firebase project.
- **Hosting URL**: The URL of the deployed version of the application. It consists of the unique identifier of the Firebase project and the `.web.app` suffix that is added automatically from Firebase.

> **Important note**
> The service worker requires an application to be served with HTTPS to work properly as a PWA, except in the **localhost** that is used for development. Firebase hosts web applications with HTTPS by default.

Now that we have deployed our application, let's see how we can install it as a PWA on our device:

1. Navigate to the hosting URL and click on the **plus** button next to the address bar of the browser:

![](img/efea53d01337e7b223389b38dcdb0160_140_0.png)

Figure 4.9 – Installing the application (Google Chrome)

> **Important note**
> The **Install** button may be found in different locations in other browsers.

The browser will prompt us to install the **weather-app** application.

## Practice questions

Let's take a look at a few practice questions:

1. What is the purpose of environment files in Angular?
2. How do we pass query parameters in an HTTP request?
3. How do we get results back from an HTTP get method?
4. Why do we use a service worker?
5. Which Angular Material component is used for notifications?
6. How are we notified regarding updates from the service worker?
7. What is the primary use of the pipe RxJS operator?
8. How can we change the theme color of a PWA application?
9. Which npm package do we use for deploying to Firebase?
10. How can we change the folder that is deployed in Firebase?

## Further reading

Add this before the bullet points:

- PWA: https://web.dev/progressive-web-apps/
- OpenWeather API: https://openweathermap.org/api
- Angular Material: https://material.angular.io/
- Angular HTTP client: https://angular.io/guide/http
- Angular service worker: https://angular.io/guide/service-worker-getting-started
- Communicating with the Angular service worker: https://angular.io/guide/service-worker-communications
- HTTP server: https://www.npmjs.com/package/http-server
- Firebase hosting: https://firebase.google.com/docs/hosting
- Deployment in Angular: https://angular.io/guide/deployment#automatic-deployment-with-the-cli

## 5 Building a WYSIWYG Editor for the Desktop using Electron

Web applications are traditionally built with HTML, CSS, and JavaScript. Their use has also been widely spread to server development using Node.js. Various tools and frameworks have emerged in recent years that use HTML, CSS, and JavaScript to create applications for desktop and mobile. In this chapter, we are going to investigate how to create desktop applications using Angular and Electron.

Electron is a JavaScript framework that is used to build native desktop applications with web technologies. If we combine it with the Angular framework, we can create fast and highly performant web applications. In this chapter, we will build a desktop WYSIWYG editor and cover the following topics:

- Adding a WYSIWYG editor library for Angular
- Integrating Electron in the workspace
- Communicating between Angular and Electron
- Packaging a desktop application

## Essential background theory and context

Electron is a cross-platform framework that is used to build desktop applications for Windows, Linux, and Mac. Many popular applications are built with Electron, such as **Visual Studio Code**, **Skype**, and **Slack**. The Electron framework is built on top of Node.js and **Chromium**. Web developers can leverage their existing HTML, CSS, and JavaScript skills to create desktop applications without learning a new language such as C++ or C#.

> **Tip**
> Electron applications have many similarities with PWA applications. Consider building an Electron application for scenarios such as advanced filesystem manipulation or when you need a more native look and feel for your application. Another use case is when you are building a complementary tool for your primary desktop product and you want to ship them together.

An Electron application consists of two processes:

- **Main**: This interacts with the native local resources using the Node.js API.
- **Renderer**: This is responsible for managing the user interface of the application.

An Electron application can have only one main process that can communicate with one or more renderer processes. Each renderer process operates in complete isolation from the others.

The Electron framework provides the `ipcMain` and `ipcRenderer` interfaces, which we can use to interact with these processes. The interaction is accomplished using **Inter-Process Communication (IPC)**, a mechanism that exchanges messages securely and asynchronously over a common channel via a Promise-based API.

## Project overview

In this project, we will build a desktop WYSIWYG editor that keeps its content local to the filesystem. Initially, we will build it as an Angular application using **ngx-wig**, a popular WYSIWYG Angular library. We will then convert it to a desktop application using Electron and learn how to sync content between Angular and Electron. We will also see how to persist the content of the editor into the filesystem. Finally, we will package our application as a single executable file that can be run in a desktop environment.

Build time: 1 hour.

## Getting started

The following software tools are required to complete this project:

- Angular CLI: A command-line interface for Angular that you can find at https://angular.io/cli.
- Visual Studio Code: A code editor that you can download from https://code.visualstudio.com/Download.
- GitHub material: The code for this chapter can be found in the Chapter05 folder at https://github.com/PacktPublishing/Angular-Projects-Second-Edition.

## Adding a WYSIWYG editor library for Angular

We will kick off our project by creating a WYSIWYG editor as a standalone Angular application first. Use the Angular CLI to create a new Angular application from scratch:

```bash
ng new my-editor --defaults
```

We pass the following options to the `ng new` command:

- `my-editor`: Defines the name of the application
- `--defaults`: Defines CSS as the preferred stylesheet format of the application and disables routing because our application will consist of a single component that will host the editor

A WYSIWYG editor is a rich text editor, such as Microsoft Word. We could create one from scratch using the Angular framework, but it would be a time-consuming process, and we would only re-invent the wheel. The Angular ecosystem contains a wide variety of libraries to use for this purpose. One of them is the ngx-wig library, which has no external dependencies, just Angular! Let's add the library to our application and learn how to use it:

1. Use the npm client to install ngx-wig from the npm package registry:

```bash
npm install ngx-wig
```

2. Open the app.module.ts file and add NgxWigModule into the imports array of the @NgModule decorator:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { NgxWigModule } from 'ngx-wig';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    NgxWigModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

NgxWigModule is the main module of the ngx-wig library.

3. Create a new Angular component that will host our WYSIWYG editor:

```bash
ng generate component editor
```
