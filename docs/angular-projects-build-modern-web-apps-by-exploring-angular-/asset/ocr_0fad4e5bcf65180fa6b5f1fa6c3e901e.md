The UiControlsModule class is already exported from the public-api.ts file. So, when our Angular application imports UiControlsModule, it will also have our component available to use.

2. Use the generate command of the Angular CLI to create an interface for the structure of card data, as follows:

```
ng generate interface card --project=ui-controls
```

The preceding command will create a card.ts file in the ui-controls project of our workspace.

3. Open the card.ts file and add the following properties to the Card interface:

```
export interface Card {
  title: string;
  description: string;
  link: string;
}
```

4. Open the public-api.ts file and add the following export statements to make the component and the interface available to the library consumers:

```
export * from './lib/card-list/card-list.component';
export * from './lib/card';
```

5. Open the card-list.component.ts file and use the @Input decorator to define an Input property, as follows:

```
import { Component, Input, OnInit } from '@angular/core';
import { Card } from '../card';

@Component({
  selector: 'lib-card-list',
  templateUrl: './card-list.component.html',
  styleUrls: ['./card-list.component.css']
})
export class CardListComponent implements OnInit {
  @Input() cards: Card[] = [];

  constructor() { }
}
```