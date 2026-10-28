In the preceding snippet, we create the container element for the basic features of our application. The element with the col-sm-3 class selector will display the personal information feature. The element with the col-sm-9 class selector will be split into two rows, each one for the repositories and organizations feature. The resulting layout of the content will look like the following:

![](bbox=[0.32, 0.22, 0.695, 0.45])

Figure 7.1 – Main content

+   8. Open the app.component.scss file and add the following CSS styles for the header and the content of our application:

```
.toolbar {
  height: 60px;
  background-color: #1976d2;
  color: white;
  font-weight: 600;
}

.toolbar img {
  margin: 0 16px;
}

.toolbar i {
  font-size: 1.5rem;
  color: white;
  margin: 0 16px;
}
```