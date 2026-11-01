```
```js
const newItem = {
  id: newIndex,
  title: txtTitle.value,
  description: txtDescription.value,
  userId: this.user.uid
};
this.notes.push(newItem);
db.push(newItem);

txtTitle.value = '';
txtDescription.value = '';
formDialog.close();
}
},
...
```
```

Now the data have a relationship between each note and the user that created that note (Figure 10-6).

![](bbox=[0.11, 0.5, 0.85, 0.86])

Figure 10-6. Firebase web console database data