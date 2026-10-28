The MatCardModule class is an Angular Material module that exposes all components that we need for creating a card component.

+   3. Open the map.component.ts file and add the following import statements:

```
import { Component, OnInit, ViewChild } from
'@angular/core';
import { Store } from '@ngrx/store';
import { PoiSelectors } from '@packt/poi';
import { MapInfoWindow, MapMarker } from
'@angular/google-maps';
```

+   4. Declare a component property to get a reference to the information window using the @ViewChild decorator:

```
@ViewChild(MapInfoWindow) info: MapInfoWindow |
    undefined;
```

+   5. Create a method for opening the information window:

```
showInfo(marker: MapMarker) {
    this.info?.open(marker);
}
```

In the preceding code, we call the open method of the information window reference, passing the associated map marker as a parameter.

+   6. Open the map.component.html file and bind the showInfo component method to the mapClick event of the map-marker component:

```
<map-marker #marker="mapMarker" [position]="poi" (mapClick)="showInfo(marker)"></map-marker>
```

We create the marker template reference variable to get a reference to the mapMarker object and pass it as a parameter in the showInfo method.

+   7. Run the application using the Serve option of Nx Console and select a POI from the list.