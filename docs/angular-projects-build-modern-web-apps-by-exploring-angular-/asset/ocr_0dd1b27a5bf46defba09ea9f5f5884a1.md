The global state of the application does not currently know when a POI has been selected. We need to add the necessary code for setting the selected POI and interacting with the store:

1. Open the poi.actions.ts file and add a new action for passing the ID of the selected POI:

```
export const selectPoi = createAction(
  '[Poi/API] Select Poi',
  props<{ poiId: string | number }>()
);
```

2. Open the poi.reducer.ts file and add a new statement in the poiReducer property that will listen to the selectPoi action and save the selected POI in the store:

```
const poiReducer = createReducer(
  initialState,
  on(PoiActions.init, (state) => ({ ...state, loaded: false, error: null })),
  on(PoiActions.loadPoiSuccess, (state, { poi }) =>
    poiAdapter.setAll(poi, { ...state, loaded: true })
  ),
  on(PoiActions.loadPoiFailure, (state, { error }) =>
    ({ ...state, error })),
  on(PoiActions.selectPoi, (state, { poiId }) => ({
    ...state, selectedId: poiId }))
);
```

3. Open the poi-list.component.ts file and import the PoiEntity interface:

```
import { Component, OnInit } from '@angular/core';
import { Store } from '@ngrx/store';
import { PoiActions, PoiEntity, PoiSelectors } from '@packt/poi';
```