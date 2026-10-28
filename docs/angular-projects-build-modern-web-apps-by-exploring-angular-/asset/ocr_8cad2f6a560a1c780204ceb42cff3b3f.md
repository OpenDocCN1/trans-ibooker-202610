# Installing the Scully library

We will use the add command of the Angular CLI to install Scully in our Angular application:

```
ng add @scullyio/init
```

The preceding command downloads and installs all the necessary npm packages for Scully to work correctly in our Angular application. It also modifies the structure of the Angular project to accommodate its specific needs.

It imports `ScullyLibModule` in our main application module:

# app.module.ts

```
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ContactModule } from './contact/contact.module';
import { CoreModule } from './core/core.module';
import { SharedModule } from './shared/shared.module';
import { ScullyLibModule } from '@scullyio/ng-lib';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    CoreModule,
    SharedModule,
    ContactModule,
    ScullyLibModule
  ],
  providers: [],
```
```