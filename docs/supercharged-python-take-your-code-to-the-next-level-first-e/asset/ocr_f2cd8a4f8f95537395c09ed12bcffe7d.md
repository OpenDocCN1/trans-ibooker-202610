> > Note
> > If there's any chance that an instance of the class might be combined in an operation
> > with another type, and if that type does have code supporting the interaction, then a
> > binary-operation method should return `NotImplemented` whenever it doesn't
> > support the types; this gives the operand on the right side of the operation a chance
> > to implement the operation. See Section 9.10.6 for more information.

# ## 9.10.5 Unary Arithmetic Methods

The methods in this category, similar to the ones listed in
Section 9.9, tend to be implemented by classes that encapsulate
some kind of number; however, they can also refer to other
mathematical objects such as a point or matrix.

Table 9.4 lists the unary arithmetic methods.

# ### Table 9.4. Magic Methods for Unary Arithmetic Operators

| Method name | Description |
|---|---|
| `\_\_pos\_\_(self)` | Unary positive sign. This method is invoked when the plus sign (+) is applied to a single operand. It’s rare that this operator ever does much besides return the object just as it is, because it is supported for the sake of completeness. |
| `\_\_neg\_\_` | Unary negative sign. This method is invoked when the negative sign (-) is applied to a single operand. |