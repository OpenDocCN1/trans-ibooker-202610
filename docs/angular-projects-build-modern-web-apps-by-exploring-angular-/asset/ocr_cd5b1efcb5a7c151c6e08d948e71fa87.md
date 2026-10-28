The card list component has now acquired drag-and-drop superpowers. Let's give it a try, as follows:

+   1. Open the app.component.html file and add an event binding to the cardChange event of the lib-card-list component, as follows:

```
<div class="container is-fluid">
<h1 class="title">Assassins Creed Series</h1>
<lib-card-list [cards]="cards"
(cardChange)="onCardChange($event)">
</lib-card-list>
</div>
```

+   2. Open the app.component.ts file and create an onCardChange method to log the new card list, as follows:

```
onCardChange(cards: Card[]) {
  console.log(cards);
}
```

+   3. Run the following command to build the library:

```
ng build ui-controls
```

+   4. Execute the serve command of the Angular CLI to start your application, like this:

```
ng serve
```

+   5. Try to drag and drop some of the cards from the list and notice the output in the Console window of your browser.

The first component of our UI library is now packed with all the functionality to make it a drag-and-drop list. It can display a list passed from our Angular application in a Bulma card format. It can also change the order of each item in the list using the Angular CDK drag-and-drop module, and propagate the change back to our application.

In the following section, we will create a second component of our library for copying data to the clipboard.