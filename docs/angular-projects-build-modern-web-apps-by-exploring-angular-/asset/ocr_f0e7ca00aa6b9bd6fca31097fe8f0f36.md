+   3. Click on the **CREATE** button, and the new issue should appear in the table:

![](bbox=[0.15, 0.14, 0.86, 0.22])

Figure 3.3 – Pending issues

+   4. Repeat steps 2 and 3 without filling in any details, and you will notice that an empty issue is added to the table.

An empty issue can be created because we have not defined any required fields yet on our report issue form. In the following section, we will learn how to accomplish this task and add validations to our form to avoid unexpected behaviors.

# # Validating the details of an issue

When we create an issue with the report issue form, we can leave the value of a form control empty since we have not added any validation rules yet. To add validations in a form control, we use the Validators class from the @angular/forms npm package. A validator is added in each form control instance that we build using the FormBuilder service. In this case, we will use the required validator to indicate that a form control is required to have a value:

+   1. Open the issue-report.component.ts file and import Validators from the @angular/forms npm package:

```
import { FormBuilder, FormGroup, Validators } from
'@angular/forms';
```

+   2. Set the Validators.required static property in all controls except the description of the issue:

```
ngOnInit(): void {
  this.issueForm = this.builder.group({
    title: ['', Validators.required],
    description: [''],
    priority: ['', Validators.required],
    type: ['', Validators.required]
  });
}
```