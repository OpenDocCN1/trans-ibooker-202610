3. Open the `environment.ts` file in the `src\environments` folder and add a new property to the environment object:

```
```typescript
export const environment = {
  production: false,
  username: '<Your GitHub login>'
};
```
```

Replace the value of the username property with your GitHub login. The environment object is used to define application-wide properties such as configuration settings or the URL of a backend API.

4. Open the `environment.prod.ts` file that exists in the same folder and add the same property as in step 3. The `environment.prod.ts` file is used when we run an Angular CLI application in production mode. The `environment.ts` file is the default one, and it is used in development mode.

> > **Important note**
> > The environment object must have the same structure and properties in all environment files.

5. Open the `app.component.ts` file and create a property in the `AppComponent` class to get the username property from the environment object:

```
```typescript
import { Component } from '@angular/core';
import { environment } from
  '../environments/environment';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  username = environment.username;
}
```
```