
```
exports: [
  UiControlsComponent,
  CardListComponent,
  CopyButtonComponent
]
```

+   5. Open the copy-button.component.ts file and declare the following component properties:

```typescript
import { Component, EventEmitter, Input, OnInit, Output }
from '@angular/core';

@Component({
  selector: 'lib-copy-button',
  templateUrl: './copy-button.component.html',
  styleUrls: ['./copy-button.component.css']
})
export class CopyButtonComponent implements OnInit {

  @Input() data = '';
  @Output() copied = new EventEmitter();

  constructor() { }

  ngOnInit(): void {
  }

}
```

The data property will be used to set clipboard data, and the copied event will fire when the data is successfully copied to the clipboard.

+   6. Create a component method to trigger a copied output event, as follows:

```typescript
onCopy() {
  this.copied.next();
}
```

+   7. Open the copy-button.component.html file and replace its content with the following HTML template:

```
<button
  class="button is-light is-primary"
  [cdkCopyToClipboard]="data"
  (cdkCopyToClipboardCopied)="onCopy() ">
    Copy
</button>
```

In the preceding template, we use a Bulma button element and attach two Angular CDK bindings to it. The cdkCopyToClipboard property binding indicates that the data component property will be copied to the clipboard when the button is clicked. The cdkCopyToClipboardCopied event binding will call the onCopy component method as soon as data has been copied to the clipboard successfully.

Now that we have set up our component, let's find out how to use it in our Angular application, as follows:

+   1. Open the app.component.html file and add a div element that consists of an input HTML element and a lib-copy-button component, as follows:

```
<div class="container is-fluid">
  <h1 class="title">Assassins Creed Series</h1>
  <lib-card-list [cards]="cards"
    (cardChange)="onCardChange($event)">
  </lib-card-list>
  <h1 class="title mt-5">Clipboard interaction</h1>
  <div class="field has-addons">
    <div class="control">
      <input class="input" type="text"
        [(ngModel)]="title">
    </div>
    <div class="control">
      <lib-copy-button [data]="title"
        (copied)="log()"></lib-copy-button>
    </div>
  </div>
</div>
```

In the previous template, we bind the `title` property of the component to the `input` element using the `ngModel` directive. We also bind it to the `data` property of the `lib-copy-button` component to copy the contents of the `input` element to the clipboard. We also bind the `copied` event to the `log` component method.

2. Open the `app.component.ts` file and create a `log` method for displaying an information message when data is copied to the clipboard, as follows:

```
log() {
  alert(this.title + ' copied to the clipboard');
}
```

3. Open the `app.module.ts` file and import `FormsModule`, like this:

```
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    UiControlsModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

The `FormsModule` class is part of the `@angular/forms` npm package and is required when we want to use `ngModel` in our application.

4. Execute the following command to build the library so that our application can recognize the new component:

```
ng build ui-controls
```

+   5. Run the application using the `serve` command of the Angular CLI, and you should get the following output:

![](img/efea53d01337e7b223389b38dcdb0160_308_0.png)

Figure 9.3 – Clipboard interaction

+   6. Enter `my awesome library` into the textbox and click on the **Copy** button, as illustrated in the following screenshot:

![](img/efea53d01337e7b223389b38dcdb0160_308_1.png)

Figure 9.4 – Alert message

We have successfully created a button that we can attach to an Angular application and interact with the clipboard directly!

The Angular CDK contains various other components and behaviors that we can use in our Angular applications. When combined with a highly customizable CSS framework such as Bulma, it can create compelling and unique interfaces. Try them in your Angular projects and build a library with a rich set of components. In the following section, we will learn how to publish a library in the npm package registry.

## Publishing an Angular library to npm

We have already seen how to build an Angular library and consume it in an Angular application when both exist in the same repository or organization. However, there are cases where you may want to make your library available to Angular projects outside your infrastructure via a public package registry such as npm. A usual case is when you want to make your library open source so that other members in the development community can benefit from this. Let’s see how to publish our `ui-controls` library to npm, as follows:

+   1. If you do not have an npm account, navigate to https://www.npmjs.com/signup to create one. Otherwise, continue to Step 3.

2. Enter the following details and click the **Create an Account** button:

## Sign Up

Username

Email address

Password

**Note:** Your email address will be added to the metadata of packages that you publish, so it may be seen publicly.

Your password should be at least 10 characters. [Learn more]()

Agree to the [End User License Agreement]() and the [Privacy Policy]().

Create an Account

or, Login

Figure 9.5 – Creating an npm account

After your account has been created, you will be redirected to your profile page in npm.

3. Open the package.json file that exists in the projects\ui-controls folder of the Angular CLI workspace and change the version of your library accordingly, as follows:

```
{
  "name": "ui-controls",
  "version": "1.0.0",
  "peerDependencies": {
    "@angular/common": "^12.0.2",
    "@angular/core": "^12.0.2",
```

Well done! Your library is now on the public npm registry and can be used by other developers in their Angular applications.

> **Important note**
> Always remember to change the version number in the package.json file of your library before publishing it. Otherwise, the npm registry will throw an error, stating that the version you are trying to publish already exists. There are also some npm packages such as standard-version that can automate this for you.

In the following section, we will learn how to use our library in non-Angular applications using Angular elements.

## Using components as Angular elements

We have already learned how to use the Angular CLI for creating an Angular library. We also saw how to publish our library to the npm registry so that other Angular projects can use it and benefit from it. In this section, we will go the extra mile and learn how to build our Angular library to be used in non-Angular environments.

As we have already pointed out, the Angular framework is a cross-platform JavaScript framework in many ways. It can run on the server using Angular Universal, and on mobile platforms. It can also run on a native desktop environment. In addition to those platforms, it can even run on web applications that are not built with Angular, using Angular elements.

Let's see how we can convert our clipboard component to an Angular element, as follows:

+   1. Execute the following Angular CLI command to generate a new Angular application in our workspace:

```
ng generate application ui-elements --defaults
```

The preceding command will generate the ui-elements Angular application in the projects folder using default options.

> **Important note**
> The Angular CLI does not currently support the use of Angular elements directly on an Angular library. Thus, we need to create an Angular application whose only purpose will be to export our components as Angular elements.

2. Run the following add schematic of the Angular CLI to install the Angular Elements package in the ui-elements application:

```
ng add @angular/elements --project=ui-elements
```

The @angular/elements npm package contains all the necessary artifacts for working with Angular elements. The installation will update the package.json file of our workspace with the latest version. It will also import the document-register-element library into the polyfills.ts file of the ui-elements application for browsers that do not support custom elements.

3. Open the app.module.ts file of the ui-elements application and remove any references to the AppComponent class.

4. Add the following import statements at the top of the app.module.ts file:

```
import { Injector, NgModule } from '@angular/core';
import { createCustomElement } from '@angular/elements';
import { BrowserModule } from '@angular/platform-browser';
import { UiControlsModule, CopyButtonComponent } from 'ui-controls';
```

5. Add the UiControlsModule class to the imports array of the @NgModule decorator, as follows:

```
@NgModule({
    declarations: [],
    imports: [
        BrowserModule,
        UiControlsModule
    ],
    providers: [],
    bootstrap: []
})
```

6. Add a constructor to the AppModule class and inject the Injector service, as follows:

```
constructor(private injector: Injector) {}
```
