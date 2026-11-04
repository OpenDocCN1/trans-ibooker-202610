# Working with FFI creational methods

The FFI methods that fall into the creational category are designed to produce either FFI instances directly or instances of classes provided by the FFI extension. When working with C functions made available through the FFI extension, it's important to recognize that you cannot directly pass native PHP variables into the function and expect it to work. The data must first be either created as a FFI data type or imported into a FFI data type, before the FFI data type can be passed into the C function. In order to create a FFI data type, use one of the functions summarized in Table 4.1, shown here:

| FFI Method | Returns | Notes |
|---|---|---|
| `arrayType()` | `FFI\CType` | Produces a C array. The first argument is the data type produced by a call to `FFI::type()`. The second argument is an array representing the dimensions of the array. |
| `cdef()` | `FFI` | As an argument you must provide a C language code block. The FFI instance returned provides a bridge between the PHP program and the C language library. |
| `new()` | `FFI\CData` | Produces a native C data structure. If the first argument is a string, it must be a valid C type. Otherwise, supply an `FFI\CType` object as the first argument. |
| `scope()` | `FFI` | Creates an FFI instance based upon *pre-loaded* C declarations. |
| `string()` | `string` | Returns a native PHP string from an `FFI\CData` instance consisting of given number of bytes. |
| `type()` | `FFI\CType` | Creates an `FFI\CType` object that can be used with other FFI extension classes. |

Table 4.1 – Summary of FFI class creational methods

Both the cdef() and scope() methods produce a direct FFI instance, while the other methods produce object instances that can be used to create a FFI instance. string() is used to extract a given number of byes from a native C variable. Let's have a look at creating and using FFI\CType instances.

# Creating and using FFI\CType instances

It's extremely important to note that once the FFI\CType instance has been created, do not simply assign a value to it as if it were a native PHP variable. Doing so would simply overwrite the FFI\CType instance due to the fact that PHP is loosely typed. Instead, to assign a scalar value to a FFI\CType instance, use its cdata property.