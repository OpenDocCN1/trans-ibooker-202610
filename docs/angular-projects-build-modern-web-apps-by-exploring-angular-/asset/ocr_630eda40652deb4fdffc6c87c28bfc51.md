+   7. Implement an ngDoBootstrap hook method to create the custom element for the CopyButtonComponent class, as follows:

```
export class AppModule {

  constructor(private injector: Injector) {}

  ngDoBootstrap() {
    const el =
      createCustomElement(CopyButtonComponent,
        { injector: this.injector });
    customElements.define('copy-button', el);
  }
}
```

The ngDoBootstrap method is used to hook in the manual bootstrap process of the Angular application. We use the createCustomElement method from the @angular/elements npm package to create a custom element, passing the class of the component and the injector. Finally, we use the define method of the customElements object to declare the custom element, passing the HTML selector that we want to use and the custom element as parameters.

Now that we have put into practice all the workings for converting an Angular component into an Angular element, it's time to build it so that we can use it in a web application.

Building an Angular element differs from a standard build of an Angular application. When we build an Angular application, the Angular CLI generates different JavaScript bundles that contain the application source code, the Angular framework, and any third-party libraries. In an Angular element scenario, we only want to generate one bundle file containing our component. For this purpose, we will use the ngx-build-plus library, which can generate a single bundle, among other things. Let's see how to install it and use it in our application, as follows:

+   1. Execute the following command of the Angular CLI to install the ngx-build-plus package:

```
ng add ngx-build-plus --project=ui-elements
```

The preceding command will modify the angular.json file of the Angular CLI workspace to use the ngx-build-plus library for building the ui-elements application.