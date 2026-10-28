```
}

onCloseReport() {
  this.showReportIssue = false;
  this.getIssues();
}

onConfirm(confirmed: boolean) {
  if (confirmed && this.selectedIssue) {
    this.issueService.completeIssue(this.selectedIssue);
    this.getIssues();
  }
  this.selectedIssue = null;
}

private getIssues() {
  this.issues = this.issueService.getPendingIssues();
}
}
```

The onConfirm method calls the completeIssue method of the issueService property only when the confirmed parameter is true. In this case, it also calls the getIssues method to refresh the table data. The selectedIssue property holds the issue object that we want to resolve, and it is reset whenever the onConfirm method is called.

+   3. Open the issue-list.component.html file and add an action overflow component inside the clr-dg-row component:

```
<clr-dg-row *clrDgItems="let issue of issues">
  <clr-dg-action-overflow>
    <button class="action-item" (click)="selectedIssue = issue">Resolve</button>
  </clr-dg-action-overflow>
  <clr-dg-cell>{{issue.issueNo}}</clr-dg-cell>
  <clr-dg-cell>{{issue.type}}</clr-dg-cell>
```