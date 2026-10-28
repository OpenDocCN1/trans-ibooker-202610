```
```typescript
    type: ['', Validators.required]
  });
  this.issueForm.controls.title.valueChanges.subscribe((
    title: string) => {
      this.suggestions =
        this.issueservice.getSuggestions(title);
    });
  }
```
```

Each control exposes a valueChanges observable that we can subscribe to and get a continuous stream of values. The valueChanges observable emits new values as soon as the user starts typing in the title control of the form. We set the result of the getSuggestions method in the suggestions component property when that happens.

+   5. To display the suggested issues on the template of the component, open the `issue-report.component.html` file and add the following HTML code right after the `clr-input-container` element:

```
```html
<div class="clr-row" *ngIf="suggestions.length">
  <div class="clr-col-lg-2"></div>
  <div class="clr-col-lg-6">
    <clr-stack-view>
      <clr-stack-header>Similar issues
      </clr-stack-header>
      <clr-stack-block *ngFor="let issue of
        suggestions">
        <clr-stack-label>#{{issue.issueNo}}:
          {{issue.title}}</clr-stack-label>
        <clr-stack-content>{{issue.description}}
        </clr-stack-content>
      </clr-stack-block>
    </clr-stack-view>
  </div>
</div>
```
```