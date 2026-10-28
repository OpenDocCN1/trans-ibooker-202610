
-   4. Open the `index.html` file and have a look again at the `app-root` element.

The main component is not empty this time. Angular Universal has made all HTTP requests to the GitHub API and prefetched the content of our application during runtime. All component templates and styles have been prerendered in the main HTML file, which essentially means that we can view our application on a browser even without JavaScript enabled!

-   5. Execute the following command to start the prerendered version of our GitHub portfolio application:

```
npm run serve:ssr
```

The preceding command will start the application at http://localhost:4000.

-   6. Disable JavaScript from the settings of your browser and navigate to http://localhost:4000.

The GitHub portfolio application that we created remains fully operational without having JavaScript enabled. The main page of the application is also rendered instantly without having the user wait for the application to load.

The previous scenario is a perfect fit for users who cannot afford to have JavaScript enabled on their devices. But what happens when the same prerendered version of the application is used by a user with JavaScript enabled? Let's learn more about that:

-   1. Enable JavaScript in your browser and toggle the developer tools.

-   2. Navigate to http://localhost:4000. Nothing different seems to happen at first sight. Nevertheless, the application loads instantly due to the prerendered content.

-   3. Inspect the Network tab and you will notice the following:

![](img/efea53d01337e7b223389b38dcdb0160_239_0.png)

Figure 7.6 – Network tab (Google Chrome)

Our application initiates all HTTP requests to the GitHub API as if it was rendered from a browser. It essentially duplicates all HTTP requests needed from the application even if data has already been prerendered on the HTML page. Why is that?

The application makes one HTTP request for the browser rendered version and another for the SSR application because both versions have a different state. We can prevent the previous behavior by sharing the state between the server and the browser. More specifically, we can transfer the state of the server to the browser using a special-purpose Angular module of the Angular Universal library called TransferHttpCacheModule.

If we use TransferHttpCacheModule, the server will cache responses from the GitHub API and the browser will use the cache instead of initiating a new request. TransferHttpCacheModule solves the problem by installing an HTTP interceptor in the Angular application that ignores HTTP requests that have been handled by the server initially.

> **Important note**
> An HTTP interceptor is an Angular service that intercepts HTTP requests and responses that originate from the built-in HTTP client of the Angular framework.

To install TransferHttpCacheModule in our GitHub portfolio application, follow these steps:

-   1. Open the main module file of the Angular application, app.module.ts, and import TransferHttpCacheModule from the @nguniversal/common npm package:

```
import { TransferHttpCacheModule } from '@nguniversal/common';
```

-   2. Add the TransferHttpCacheModule class to the imports array of the @NgModule decorator:

```
@NgModule({
  declarations: [
    AppComponent,
    PersonalInfoComponent,
    PanelComponent,
    RepositoriesComponent,
    OrganizationsComponent
],
imports: [
  BrowserModule.withServerTransition({ appId:
  'serverApp' }),
  HttpClientModule,
  TransferHttpCacheModule
],
providers: [],
bootstrap: [AppComponent]
})
```

-   3. Open the main module file of the SSR application, app.server.module.ts, and import ServerTransferStateModule from the @angular/platform-server npm package:

```
import { ServerModule, ServerTransferStateModule }
from '@angular/platform-server';
```

-   4. Add the ServerTransferStateModule class to the imports array of the @NgModule decorator:

```
@NgModule({
  imports: [
    AppModule,
    ServerModule,
    ServerTransferStateModule
  ],
  bootstrap: [AppComponent],
})
```

-   5. Execute the following command to prerender your application:

```
npm run prerender
```

-   6. Run the following command to start your prerendered application:

```
npm run serve:ssr
```

If you preview the portfolio application and inspect the **Network** tab of your browser, you will notice that it does not make additional HTTP requests. `TransferHttpCacheModule` intercepted all HTTP requests and stored them in the `TransferState` store of our application. TransferState is a key-value store that can be transferred from the server to the browser. The browser version of the application later can read the HTTP responses directly from the store without making an extra call.

We now have a fully prerendered version of our GitHub portfolio. But how can we optimize it further so that we can share it on a social media platform? We will learn more about SEO optimization techniques in the following section.

## Enhancing SEO capabilities

**SEO** is the process of optimizing a website to be correctly indexed from a web crawler. A web crawler is a special-purpose software that is present on most search engines and can identify and index websites so that they are easily discoverable and linkable through their platforms.

Angular Universal does a great job of SEO by prerendering content during build time. Some web crawlers cannot execute JavaScript and build the dynamic content of an Angular application. Prerendering with Angular Universal eliminates the need for JavaScript, thus allowing web crawlers to do their best to identify the web application.

We can also help SEO by defining several tags in the `head` element of the main `index.html` file of an Angular application, such as `title`, `viewport`, and `charset`:

## index.html

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>GhPortfolio</title>
  <base href="/">
  <meta name="viewport" content="width=device-width,
    initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>

<app-root></app-root>
</body>
</html>
```

You can find a list of available tags at https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name.

However, setting a tag in the index.html file is not adequate, especially when an Angular application has routing enabled and contains several routes. The Angular framework provides a couple of handy services that we can use to set tags programmatically. First, let's see how to set the title tag in our application:

-   1. Open the app.component.ts file and import the Title and OnInit Angular artifacts:

```
import { Component, OnInit } from '@angular/core';
import { environment } from
'../environments/environment';
import { Title } from '@angular/platform-browser';
```

-   2. Inject the Title service into the constructor of the AppComponent class:

```
export class AppComponent {
  username = environment.username;

  constructor(private title: Title) {}
}
```

-   3. Add the OnInit interface to the implemented interface list of the component and call the setTitle method of the title variable in the ngOnInit method:

```
export class AppComponent implements OnInit {
  username = environment.username;

  constructor(private title: Title) {}

  ngOnInit() {
    this.title.setTitle('GitHub portfolio app');
  }
}
```

-   4. Run `npm run dev:ssr` to preview the application, and you should see the title in the browser tab:

![](img/efea53d01337e7b223389b38dcdb0160_244_0.png)

Figure 7.7 – Browser tab title

Similar to the Title service, we can use the Meta service to set meta tags for our application:

-   1. Open the `app.component.ts` file and import `Meta` from the `@angular/platform-browser` npm package:

```
import { Meta } from '@angular/platform-browser';
```

-   2. Inject the `Meta` service into the constructor of the `AppComponent` class:

```
constructor(private title: Title, private meta: Meta) {}
```

-   3. Use the `addTags` method of the `meta` variable to add some meta tags to the `ngOnInit` method:

```
ngOnInit(): void {
  this.title.setTitle('GitHub portfolio app');
  this.meta.addTags([
    {
      name: 'description',
      content: `${this.username}'s GitHub portfolio`
    },
    {
      name: 'author',
      content: this.username
    }
  ]);
}
```

In the preceding code, we add two meta tags. The first one sets the description that contains the username of the current GitHub profile. The second one sets the author tag to be the same as the username of the GitHub profile.
