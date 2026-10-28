The Angular CLI will create the admin component inside the folder of the admin library. We now need to wire up the admin library to the monorepo application:

1. Open the app.module.ts file and add a new route configuration object for the admin library:

```
RouterModule.forRoot([
  { path: 'admin', loadChildren: () =>
    import('@packt/admin').then(m => m.AdminModule) },
  { path: 'tour', loadChildren: () =>
    import('@packt/visitor').then(m => m.VisitorModule)
  },
  { path: '', pathMatch: 'full', redirectTo: 'tour' }
])
```

2. Open the admin.module.ts file and add a route configuration to activate AdminComponent by default:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AdminComponent } from './admin.component';
import { RouterModule } from '@angular/router';

@NgModule({
  imports: [
    CommonModule,
    RouterModule.forChild([
      { path: '', component: AdminComponent }
    ]),
  ],
  declarations: [
    AdminComponent
  ],
})
export class AdminModule {}
```

3. Use the Serve option of Nx Console to run the application and navigate to http://localhost:4200.