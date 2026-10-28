
4. Try to refresh your browser. You will see an indication that you are disconnected from the internet, as shown in the following screenshot:

    ![](img/efea53d01337e7b223389b38dcdb0160_127_0.png)

    Figure 4.4 – No internet connection (Google Chrome)

The previous case is pretty standard in areas with low-quality internet connections. So, what can we do for our users in such areas? Luckily, the Angular framework contains an implementation of a service worker that can significantly enhance the UX of our application when running in offline mode. It can cache certain parts of the application and deliver them accordingly instead of making real requests.

> **Tip**
> The Angular service worker can also be used in environments with large network latency connections. Consider using a service worker in this type of network to also improve the experience of your users.

Run the following command of the Angular CLI to enable the service worker in our Angular application:

```
ng add @angular/pwa
```

The preceding command will transform the Angular CLI workspace accordingly for PWA support:

- It adds the `@angular/service-worker` npm package to the dependencies section of the `package.json` file of the application.
- It creates the `manifest.webmanifest` file in the `src` folder of the application. The manifest file contains information about the application needed to install and run the application as a native one. It also adds it to the `assets` array of the `build` options in the `angular.json` file.
- It creates the `ngsw-config.json` file at the root of the project, which is the service worker configuration file. We use it to define configuration-specific artifacts, such as which resources are cached and how they are cached. You can find more details about the configuration of the service worker at the following link: https://angular.io/guide/service-worker-config#service-worker-configuration.

    The configuration file is also set in the `ngswConfigPath` property of the `build` configuration in the `angular.json` file.

- It sets the `serviceWorker` property to `true` in the `build` configuration of the `angular.json` file.
- It registers the service worker in the `app.module.ts` file:

    ```typescript
    import { ServiceWorkerModule } from '@angular/service-worker';
    import { environment } from '../environments/environment';

    @NgModule({
      declarations: [
        AppComponent,
        WeatherComponent
      ],
      imports: [
        BrowserModule,
        BrowserAnimationsModule,
        HttpClientModule,
        MatIconModule,
        MatInputModule,
        MatCardModule,
        ServiceWorkerModule.register('ngsw-worker.js', {
          enabled: environment.production,
          // Register the ServiceWorker as soon as the app is
          // stable
          // or after 30 seconds (whichever comes first).
          registrationStrategy: 'registerWhenStable:30000'
        })
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```

The ngsw-worker.js file is the JavaScript file that contains the actual implementation of the service worker. It is created automatically for us when we build our application in production mode. Angular uses the register method of the ServiceWorkerModule class to register it within our application.

- It creates several icons to be used when the application is installed as a native one in the device of the user.
- It includes the manifest file and a meta tag for theme-color in the head of the index.html file:

    ```
    <link rel="manifest" href="manifest.webmanifest">
    <meta name="theme-color" content="#1976d2">
    ```

Now that we have completed the installation of the service worker, it is time to test it! Before moving on, we should install an external web server because the built-in function of the Angular CLI does not work with service workers. A good alternative is http-server:

1. Run the install command of the npm client to install http-server:

    ```
    npm install -D http-server
    ```

    The preceding command will install http-server as a development dependency of our Angular CLI project.

2. Build the Angular application using the build command of the Angular CLI:

    ```
    ng build
    ```

3. Open the `package.json` file of the Angular CLI workspace and add the following entry to the `scripts` property:

    ```json
    "scripts": {
        "ng": "ng",
        "start": "ng serve",
        "build": "ng build",
        "watch": "ng build --watch --configuration development",
        "test": "ng test",
        "server": "http-server -p 8080 -c-1 dist/weather-app"
    }
    ```

4. Start the HTTP web server using the following command:

    ```bash
    npm run server
    ```

    The preceding command will start `http-server` at port 8080 and will have caching disabled.

5. Open your browser and navigate to `http://localhost:8080`.

    > **Tip**
    >
    > Prefer opening the page in private or incognito mode to avoid unexpected behavior from the service worker.

6. Repeat the process that we followed at the beginning of the section for switching to offline mode.

7. If you refresh the page now, you will notice that the application is working as expected.

The service worker did all the work for us, and the process was so transparent that we could not tell whether we are online or offline. You can verify that by inspecting the **Network** tab:

![](img/efea53d01337e7b223389b38dcdb0160_130_0.png)

Figure 4.5 – Service worker (offline mode)

The (**ServiceWorker**) value in the **Size** column indicates that the service worker served a cached version of our application.

We have now successfully installed the service worker and went one step closer to converting our application into a PWA. In the following section, we will learn how to notify users of the application about potential updates.

## Staying up to date with in-app notifications

When we want to apply a change in a web application, we make the change and build a new version of our application. The application is then deployed to a web server, and every user has access to the new version immediately. But this is not the case with PWA applications.

When we deploy a new version of our PWA application, the service worker must act accordingly and apply a specific update strategy. It should either notify the user of the new version or install it immediately. Whichever update strategy we follow depends on our needs. In this project, we want to show a prompt to the user and decide whether they want to update. Let's see how to implement this feature in our application:

1. Open the `app.module.ts` file and add `MatSnackBarModule` to the `imports` array of the `@NgModule` decorator:

    ```typescript
    import { MatSnackBarModule } from '@angular/material/snack-bar';

    @NgModule({
      declarations: [
        AppComponent,
        WeatherComponent
      ],
      imports: [
        BrowserModule,
        BrowserAnimationsModule,
        HttpClientModule,
        MatIconModule,
        MatInputModule,
        MatCardModule,
        MatSnackBarModule,
        ServiceWorkerModule.register('ngsw-worker.js', {
          enabled: environment.production,
          // Register the ServiceWorker as soon as the app
          // is stable
          // or after 30 seconds (whichever comes first).
          registrationStrategy: 'registerWhenStable:30000'
        })
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```

    MatSnackBarModule is an Angular Material module that allows us to interact with snack bars. A snack bar is a pop-up window that usually appears on the bottom of the page and is used for notification purposes.

2. Open the app.component.ts file and add the OnInit interface to the implemented interfaces of the AppComponent class:

    ```typescript
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.scss']
    })
    export class AppComponent implements OnInit {
      title = 'weather-app';
    }
    ```

3. Inject the MatSnackBar and SwUpdate services in the constructor of the AppComponent class:

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import { MatSnackBar } from '@angular/material/snack-bar';
    import { SwUpdate } from '@angular/service-worker';

    @Component({
      selector: 'app-root',
    ```
