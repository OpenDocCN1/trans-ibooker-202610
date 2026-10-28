+   5. Modify the `showInfo` component method so that it dispatches a `visitPoi` action to the store:

```
```typescript
showInfo(marker: MapMarker, poiId: string | number) {
  this.store.dispatch(PoiActions.visitPoi({ poiId }));
  this.info?.open(marker);
}
```
```

+   6. Finally, open the `map.component.html` file and pass the selected POI ID into the `showInfo` method:

```
```html
<map-marker #marker="mapMarker" [position]="poi"
  (mapClick)="showInfo(marker, poi.id)"></map-marker>
```
```

Our application can now record the visits of each POI and keep them in the local storage of the browser. In the following section, we will create the main component of the administrator portal that leverages visit data.

# Displaying visit statistics

The administrator portal will display visit statistics on its main component with a graph. We will use the **ng2-charts** library for visualizing data on a pie chart. Let's see how to add the required functionality in that component:

+   1. Install the ng2-charts library using the following command:

```
```bash
npm install ng2-charts chart.js@2.9.3
```
```

The preceding command will also install the `chart.js` library, which is at the core of the ng2-charts library.

+   2. Open the `admin.module.ts` file and import `PoiModule` from the `@packt/poi` namespace and `ChartsModule` from the ng2-charts npm package:

```
```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AdminComponent } from './admin.component';
import { RouterModule } from '@angular/router';
import { PoiModule } from '@packt/poi';
import { ChartsModule } from 'ng2-charts';
```
```