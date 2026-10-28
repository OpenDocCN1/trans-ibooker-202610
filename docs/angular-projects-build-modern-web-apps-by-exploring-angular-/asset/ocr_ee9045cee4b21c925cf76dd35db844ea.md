The (**ServiceWorker**) value in the **Size** column indicates that the service worker served a cached version of our application.

We have now successfully installed the service worker and went one step closer to converting our application into a PWA. In the following section, we will learn how to notify users of the application about potential updates.

# Staying up to date with in-app notifications

When we want to apply a change in a web application, we make the change and build a new version of our application. The application is then deployed to a web server, and every user has access to the new version immediately. But this is not the case with PWA applications.

When we deploy a new version of our PWA application, the service worker must act accordingly and apply a specific update strategy. It should either notify the user of the new version or install it immediately. Whichever update strategy we follow depends on our needs. In this project, we want to show a prompt to the user and decide whether they want to update. Let's see how to implement this feature in our application:

+   1. Open the `app.module.ts` file and add `MatSnackBarModule` to the `imports` array of the `@NgModule` decorator:

```
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

```