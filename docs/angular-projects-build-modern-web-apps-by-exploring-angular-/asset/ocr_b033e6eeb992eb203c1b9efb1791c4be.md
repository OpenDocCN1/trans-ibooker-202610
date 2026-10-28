# Turning on suggestions for new issues

The reactive forms API contains a mechanism for getting notified when the value of a particular form control changes. We will use it in our application to find related issues when reporting a new one. More specifically, we will display a list of suggested issues when the user starts typing in the title form control:

+   1. Open the `issues.service.ts` file and add the following method:

```
```typescript
getSuggestions(title: string): Issue[] {
  if (title.length > 3) {
    return this.issues.filter(issue =>
      issue.title.indexOf(title) !== -1);
  }
  return [];
}
```
```

The getSuggestions method takes the title of an issue as a parameter and searches for any issues that contain the same title. The search mechanism is triggered when the title parameter is more than three characters long to limit results down to a reasonable amount.

+   2. Open the `issue-report.component.ts` file and add the following import statement:

```
```typescript
import { Issue } from '../issue';
```
```

+   3. Create a new component property to hold the suggested issues:

```
```typescript
suggestions: Issue[] = [];
```
```

+   4. The controls property of a FormGroup object contains all form controls as a key-value pair. The key is the name of the control, and the value is the actual form control object of the AbstractControl type. We can get notified about changes in the value of a form control by accessing its name, in this case the title, in the following way:

```
```typescript
ngOnInit(): void {
  this.issueForm = this.builder.group({
    title: ['', Validators.required],
    description: [''],
    priority: ['', Validators.required],
```
```