An ion-list-header element is the header of a list. An ion-note element is a text element that is used to provide additional information, such as the subtitle of a list.

+   6. Open the TypeScript file of the main component, `app.component.ts`, and modify it as follows:

```
import { Component } from '@angular/core';
@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html',
  styleUrls: ['app.component.scss'],
})
export class AppComponent {
  public appPages = [
    {
      title: 'Take a photo',
      url: '/capture',
      icon: 'camera'
    },
    {
      title: 'View gallery',
      url: '/view',
      icon: 'globe'
    }
  ];
}
```

The appPages property contains all the pages of our application. Each page has a title, the URL from which it is accessible, and an icon. Our application will consist of two pages, one that will be used for taking photos using the camera and another for displaying them on a map.

+   7. Run the `serve` command of the Ionic CLI to start the application:

```
ionic serve
```

The preceding command will build your application and open your default browser at `http://localhost:8100`.