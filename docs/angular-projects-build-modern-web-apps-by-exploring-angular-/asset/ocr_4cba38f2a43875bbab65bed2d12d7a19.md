```
myContent = '';

constructor(private editorService: EditorService) {}

ngOnInit(): void {
}
}
```

7. Create a method that calls the getContent method of the editorService variable and execute it inside the ngOnInit method:

```
ngOnInit(): void {
  this.getContent();
}

private async getContent() {
  this.myContent = await
    this.editorService.getContent();
}
```

We use the async/await syntax, which allows the synchronous execution of our code in promise-based method calls.

8. Create a method that calls the setContent method of the editorService variable:

```
saveContent(content: string) {
  this.editorService.setContent(content);
}
```

9. Let's wire up those methods that we have created with the template of the component. Open the editor.component.html file and add the following bindings:

```
<ngx-wig placeholder="Enter your content"
  [ngModel]="myContent"
  (contentChange)="saveContent($event)"></ngx-wig>
```