Another use case for the FFI extension, outside of rapid prototyping, is a way to allow PHP direct access to obscure or proprietary C code. An example of this would be the custom C code written to control factory machines. In order to have PHP run the factory, the FFI extension can be used to bind PHP directly to the C libraries controlling the various machines.

Finally, another use case for this extension is to use it to preload C libraries, potentially reducing memory consumption. Before we show usage examples, let's have a look at the FFI class and its methods.

# Examining the FFI class

As you learned in this chapter, not every developer has a need to use the FFI extension. Having direct experience with the FFI extension deepens your understanding of the internals of the PHP language, and this deepened understanding can have a beneficial impact on your career as a PHP developer: it's quite possible that at some point in the future, you will be employed by a company that has developed a custom PHP extension. Knowing how to operate the FFI extension in this situation allows you to develop new features for a custom PHP extension, as well as helping you to troubleshoot extension problems.

The FFI class consists of 20 methods that fall into four broad categories, outlined as follows:

+   - **Creational**: Methods in this category create instances of classes available from the FFI extension **application programming interface** (API).
+   - **Comparison**: Comparison methods are designed to compare C data values.
+   - **Informational**: This set of methods gives you metadata on C data values, including size and *alignment*.
+   - **Infrastructural**: Infrastructural methods are used to carry out logistical operations such as copying, populating, and releasing memory.

> Tip
> The complete FFI class is documented here: https://www.php.net/manual/en/class.ffi.php.

Interestingly, all FFI class methods can be called in a static manner. It's now time to take a dive into the details and usage of the class associated with FFI, starting with the *creational* methods.