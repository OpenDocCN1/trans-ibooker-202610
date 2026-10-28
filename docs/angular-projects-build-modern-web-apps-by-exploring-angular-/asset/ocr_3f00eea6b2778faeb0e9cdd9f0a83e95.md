```
```typescript
{ path: '', component: VisitorComponent }
]),
PoiModule,
GoogleMapsModule
],
declarations: [VisitorComponent, PoiListComponent],
})
```
```

4. Open the `index.html` file of the application and include the **Google Maps JavaScript API** inside the head element:

```
```html
<script src="https://maps.googleapis.com/maps/api/js">
</script>
```
```

Now that we have installed and registered Google Maps in our application, let's create the Angular component that will host it:

+   1. Execute the following command of Nx CLI to create a new Angular component:

```
```bash
nx generate component map --project=visitor
```
```

+   2. Open the `map.component.ts` file and add the following import statements:

```
```typescript
import { Store } from '@ngrx/store';
import { PoiSelectors } from '@packt/poi';
```
```

+   3. Inject the Store service in the constructor of the MapComponent class and declare a property to get the selected POI from the store:

```
```typescript
export class MapComponent implements OnInit {

  pois = this.store.select(PoiSelectors.getSelected);

  constructor(private store: Store) { }

  ngOnInit(): void {
  }

}
```
```