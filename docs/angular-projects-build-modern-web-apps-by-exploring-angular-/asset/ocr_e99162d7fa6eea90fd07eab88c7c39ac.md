Our application initiates all HTTP requests to the GitHub API as if it was rendered from a browser. It essentially duplicates all HTTP requests needed from the application even if data has already been prerendered on the HTML page. Why is that?

The application makes one HTTP request for the browser rendered version and another for the SSR application because both versions have a different state. We can prevent the previous behavior by sharing the state between the server and the browser. More specifically, we can transfer the state of the server to the browser using a special-purpose Angular module of the Angular Universal library called TransferHttpCacheModule.

If we use TransferHttpCacheModule, the server will cache responses from the GitHub API and the browser will use the cache instead of initiating a new request. TransferHttpCacheModule solves the problem by installing an HTTP interceptor in the Angular application that ignores HTTP requests that have been handled by the server initially.

> **Important note**
> An HTTP interceptor is an Angular service that intercepts HTTP requests and responses that originate from the built-in HTTP client of the Angular framework.

To install TransferHttpCacheModule in our GitHub portfolio application, follow these steps:

+   1. Open the main module file of the Angular application, app.module.ts, and import TransferHttpCacheModule from the @nguniversal/common npm package:

```
import { TransferHttpCacheModule } from '@nguniversal/common';
```

+   2. Add the TransferHttpCacheModule class to the imports array of the @NgModule decorator:

```
@NgModule({
  declarations: [
    AppComponent,
    PersonalInfoComponent,
    PanelComponent,
    RepositoriesComponent,
    OrganizationsComponent
```