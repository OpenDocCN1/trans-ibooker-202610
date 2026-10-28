We are now ready to convert our amazing Angular application into a professional blog website. Before we move on, let's add some additional routes to the `app-routing.module.ts` file:

```
```typescript
const routes: Routes = [
  { path: 'contact', component: ContactComponent },
  { path: 'articles', loadChildren: () =>
      import('./articles/articles.module').then(m =>
      m.ArticlesModule) },
  { path: '', pathMatch: 'full', redirectTo: 'articles' },
  { path: '**', redirectTo: 'articles' }
];
```
```

We added a default route to automatically redirect our blog users to the `articles` path upon visiting the blog. Additionally, we created a new route configuration object with its path set to `**` that also navigates to the `articles` path. The `**` syntax is called the **wildcard route**, and it is triggered when the router cannot match a requested URL with a defined route.

> **Tip**
> 
> Define the most specific routes first and then add any generic ones such as the default and the wildcard routes. The Angular router parses the route configuration in the order that we define and follows a *first match wins* strategy to select one.

We have already enabled and configured routing in our Angular application. In the following section, we will establish the infrastructure needed to add blogging capabilities to our application.

# # Adding blog capabilities with Scully

Our application currently does not have any specific logic regarding blog posts. It is a typical Angular application that uses routing. However, by adding a routing configuration, we have established the foundation for adding blog support using Scully.

> **Important Note**
> 
> Scully needs *at least* one route defined in an Angular application to work correctly.

First, we need to install Scully in our application.