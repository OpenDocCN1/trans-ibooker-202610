Now that you have an idea of how anonymous class naming has changed in PHP 8, let's have a look at changes in namespace handling.

# Understanding changes in namespaces

The concept of a namespace was introduced in PHP 5.3 as a means of isolating hierarchies of classes. Unfortunately, the original algorithm that was used to parse namespace names had several flaws. In addition to being overly complicated, the way in which namespace and class names were tokenized internally was performed in an inconsistent manner, leading to unexpected errors.

Before we get into the benefits and potential backward compatible breaks, let's have a look at how the namespace tokenization process has changed.

# Discovering differences in tokenization

The tokenization process is an important part of the interpretation process and takes place when your PHP code is executed. In the process of producing byte code, the PHP program code is broken down into tokens by the PHP parsing engine.

The following table summarizes namespace tokenization changes:

| Namespace + Class | Old Tokenization | New Tokenization |
|---|---|---|
| JustClass | T_STRING | T_STRING |
| \AbsoluteClass | T_NS_SEPARATOR + T_STRING | T_NAME_FULLY_QUALIFIED |
| Some\Thing\Class | T_STRING + T_NS_SEPARATOR + T_STRING | T_NAME_QUALIFIED |
| <namespace>\Class | T_NAMESPACE + T_NS_SEPARATOR + T_STRING | T_NAME_RELATIVE |

Table 9.5 – Namespace tokenization differences in PHP 8

As you can see, PHP 8 namespace tokenization is much simpler and streamlined.

> **Tip**
> 
> For more information on parser tokens, have a look at the following documentation reference: https://www.php.net/manual/en/tokens.