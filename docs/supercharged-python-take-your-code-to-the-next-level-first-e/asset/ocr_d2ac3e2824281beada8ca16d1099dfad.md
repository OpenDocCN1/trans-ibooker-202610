, key , val ue) This is an example of modifying a value in place—that is, changing data in an existing object.

_ _de lit em_ _(s elf , key ) This method deletes the specified item, using (once again) the key value to select that item.

_ _it er_ _(s elf ) Returns an iterator for the collection object; such an iterator is an object that implements the next method for the collection. The simplest way to implement the __iter__ method, therefore, is to return self but then make sure that the class implements the __next__ method directly.

_ _ne xt_ _(s elf ) This method may be implemented either by the class itself or by a helper class designed to work with the main class. In either case, this method should return the next element in an iteration, or else raise a StopIteration exception.

_ _re ver This method, if implemented, should return an iteration of the collection that has values in reverse order.