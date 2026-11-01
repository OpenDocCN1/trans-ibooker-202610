```
<mwc-fab class="floatButton" @click="handleAdd" mini icon="add">
</mwc-fab>
<mwc-dialog id="dialog" heading="Add Note">
  <div class="formFields">
    <mwc-textfield
      id="text-title"
      outlined
      minlength="3"
      label="Title"
      required>
    </mwc-textfield>
  </div>
  <div class="formFields">
  <mwc-textfield
    id="text-description"
    outlined
    minlength="3"
    label="Description"
    required>
  </mwc-textfield>
  </div>
  <div>
  <mwc-button
    id="primary-action-button"
    slot="primaryAction"
    @click="handleAddNote">
    Add
  </mwc-button>
  <mwc-button
    slot="secondaryAction"
    dialogAction="close"
    @click="handleClose">
    Cancel
  </mwc-button>
  </div>
</mwc-dialog>
```