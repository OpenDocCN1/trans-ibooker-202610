# # (NAME OR NUMBER)

![](bbox=[0.1484, 0.1469, 0.2578, 0.2188])

Click here to view code image

```
```
{ [position] [!|r|s|a] [: spec ] }
```
```

In the syntax for print fields within a **format** string, the square brackets are not intended literally but indicate optional items. With the second item, the syntax indicates an exclamation mark followed by **r**, **s**, or **a**, but not more than one of these; we look at that syntax in the next section.

The _spec_ is a potentially complex series of formatting parameters. This chapter focuses on _spec_ beginning in Section 5.8 and explains all the possible subfields.

One of the simplest applications of this syntax is to use a lone _position_ indicator.

```
```
{ position }
```
```

The _position_ indicates which argument is being referred to by using either a number or a name. Using a _position_ indicator lets you to refer to arguments out of order.

The _position_ indicator, in turn, is either an index number or a named position:

```
```
pos_index | pos_name
```
```