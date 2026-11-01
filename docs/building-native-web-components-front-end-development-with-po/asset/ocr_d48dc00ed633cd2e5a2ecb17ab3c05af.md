```
handleClose() {
  let txtTitle = this.$el.querySelector('#text-title');
  let txtDescription = this.$el.querySelector('#text-description');
  const formDialog = this.$el.querySelector('#dialog');

  txtTitle.value = '';
  txtDescription.value = '';
  formDialog.close();
}
},
}
</script>
<style scoped>
.floatButton {
  position: fixed;
  bottom: 20px;
  right: 20px;
}

.formFields {
  margin: 15px;
}
</style>
```

Here, we are using this.$el.querySelector() to select the web components and use their methods to execute logic, such as, for example, formDialog.show() and formDialog.close(), to show and hide the mwc-dialog. Also, in handleAddNote(), we are using the uuidv4() method to generate a new index with the data that the user enters in the form and, with this.notes.push(), add the new note in the local array that we are using to keep all our notes. With these modifications, you can add notes (see Figure 9-22).