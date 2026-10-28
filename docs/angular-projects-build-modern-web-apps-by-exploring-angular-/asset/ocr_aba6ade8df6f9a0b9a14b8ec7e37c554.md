```
```typescript
bootstrap: [AppComponent]
})
export class AppModule { }
```
```

ScullyLibModule is the main module of the Scully library that contains various Angular services and directives that Scully will need.

It also creates a configuration file for the Scully library in the root folder of the Angular CLI workspace:

# **scully.my-blog.config.ts**

```
```typescript
import { ScullyConfig } from '@scullyio/scully';
export const config: ScullyConfig = {
  projectRoot: "./src",
  projectName: "my-blog",
  outDir: './dist/static',
  routes: {
  }
};
```
```

The configuration file contains information about our Angular application that Scully will need along the way:

+   - projectRoot: The path containing the source code of the Angular application
+   - projectName: The name of the Angular application
+   - outDir: The output path of the Scully generated files

> **Important Note**
> 
> The Scully output path must be different from the path that the Angular CLI outputs the bundle of your Angular application. The latter can be configured from the `angular.json` file.

+   - routes: It contains the route configuration that will be used for accessing our blog posts. Scully will populate it automatically, as we will see in the following section.

Since we have installed Scully successfully in our Angular application, we can now configure it to initialize our blog.