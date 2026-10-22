Because ownership is shared, reference counting pointers (`Rc<T>` and `Arc<T>`) have the same restrictions as shared references (`&T`). They do not give you mutable access to their contained value, since the value might be borrowed by other code at the same time.

```
error[E0596]: cannot borrow data in an `Arc` as mutable
  |
6 |     a.sort();
  |     ^^^^^^^^
```

# Borrowing and Data Races

In Rust, values can be borrowed in two ways:

*Immutable Borrowing*

Borrowing something with `&` gives an *immutable reference*. Such a reference can be copied. Access to the data it references is shared between all copies of such a reference. As the name implies, the compiler does normally not allow you to *mutate* something through such a reference, since that might affect other code that’s currently borrowing the same data.

*Mutable Borrowing*

Borrowing something with `&mut` gives a *mutable reference*. A mutable borrow guarantees it’s the only active borrow of that data, making sure that mutating the data will not change anything other code is currently looking at.

These two concepts together fully prevent *data races*: situations where one thread is mutating data while another is concurrently accessing it. Data races are generally *undefined behavior*, which means the compiler does not need to take these situations into account. It will simply assume they do not happen.