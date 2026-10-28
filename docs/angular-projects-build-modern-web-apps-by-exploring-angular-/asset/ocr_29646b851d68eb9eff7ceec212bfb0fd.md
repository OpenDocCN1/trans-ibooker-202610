+   3. Open the `contact.component.html` file and add the following HTML content:

```
```html
<div class="card mx-auto text-center border-light"
     style="width: 18rem;">
  <img src="assets/angular.png" class="card-img-top"
       alt="Angular logo">
  <div class="card-body">
    <h5 class="card-title">Angular Projects</h5>
    <p class="card-text">
      A personal blog created with the Angular
      framework and the Scully static site generator
    </p>
    <a href="https://angular.io/" target="_blank"
       class="card-link">Angular</a>
    <a href="https://scully.io/" target="_blank"
       class="card-link">Scully</a>
  </div>
</div>
```
```

In the preceding code, we used the `angular.png` image, which you can find in the `src\assets` folder of the project from the accompanying GitHub repository.

> **Tip**
> 
> The assets folder in an Angular CLI project is used for static content such as images, fonts, or JSON files.

We have already created our contact feature. The next step is to add it to the main page of our Angular application:

+   1. Open the `app-routing.module.ts` file and add a new route configuration object in the `routes` property:

```
```typescript
import { ContactComponent } from './contact/contact.component';

const routes: Routes = [
  { path: 'contact', component: ContactComponent }
];
```
```