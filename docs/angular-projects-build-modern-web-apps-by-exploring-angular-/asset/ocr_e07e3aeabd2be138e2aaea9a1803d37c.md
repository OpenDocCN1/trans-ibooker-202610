+   7. Modify the `init$` property to use the `poiService` variable:

```
```
init$ = createEffect(() =>
  this.actions$.pipe(
    ofType(PoiActions.init),
    fetch({
      run: (action) => {
        return this.poiService.getAll().pipe(
          map(pois => PoiActions.loadPoiSuccess({ poi:
            pois }))
        )
      },

      onError: (action, error) => {
        console.error('Error', error);
        return PoiActions.loadPoiFailure({ error });
      },
    })
  )
);
```
```

The effect is responsible for listening to all actions that are dispatched in the store. As soon as a `PoiActions.init` action is dispatched, the `init$` property is triggered and calls the `getAll` method of the `poiService` variable. The `init$` property knows which action to listen for by the parameters in the `ofType` operator.

> **Tip**
> The `ofType` operator can accept more than one action.

If the data is fetched successfully, the effect will dispatch a new action in the store, `PoiActions.loadPoiSuccess`, with POI data as the payload. If there is a failure getting the data, it will dispatch a `PoiActions.loadPoiFailure` action in the store.

+   8. Open the `app.module.ts` file and import `HttpClientModule` from the `@angular/common/http` namespace:

```
```
import { HttpClientModule } from
  '@angular/common/http';
```
```

Add the `HttpClientModule` class also in the `imports` array of the `@NgModule` decorator.