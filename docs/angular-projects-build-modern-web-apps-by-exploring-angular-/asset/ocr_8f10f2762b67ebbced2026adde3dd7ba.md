```
// Register the ServiceWorker as soon as the app
// is stable
// or after 30 seconds (whichever comes first).
registrationStrategy: 'registerWhenStable:30000'
})
],
providers: [],
bootstrap: [AppComponent]
})
```

MatSnackBarModule is an Angular Material module that allows us to interact with snack bars. A snack bar is a pop-up window that usually appears on the bottom of the page and is used for notification purposes.

+   2. Open the app.component.ts file and add the OnInit interface to the implemented interfaces of the AppComponent class:

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {
  title = 'weather-app';
}
```

+   3. Inject the MatSnackBar and SwUpdate services in the constructor of the AppComponent class:

```
import { Component, OnInit } from '@angular/core';
import { MatSnackBar } from '@angular/material/snack-bar';
import { SwUpdate } from '@angular/service-worker';

@Component({
  selector: 'app-root',
```