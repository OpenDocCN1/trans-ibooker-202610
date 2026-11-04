
3. Next, we will produce a list of files that contain bc_break in their name:

```
$anon->search = 'bc_break';
foreach ($anon as $fn) echo $fn . "\n";
```

4. In the next two lines, we will use instanceof to test whether the anonymous class implements OuterInterface:

```
if ($anon instanceof OuterIterator)
    echo "This object implements OuterIterator\n";
```

5. Finally, we will simply dump the contents of the anonymous class using var_dump():

```
echo var_dump($anon);
```

Here is the output running under PHP 8. We cannot run this example in PHP 7 as that version lacks the str_contains() function!

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_oop_diff_anon_class_renaming.php
php8_bc_break_construct.php
php8_bc_break_serialization.php
php8_bc_break_destruct.php
php8_bc_break_sleep.php
php8_bc_break_scanner.php
php8_bc_break_serializable.php
php8_bc_break_magic_wrong.php
php8_bc_break_magic.php
php8_bc_break_magic_to_string.php
This object implements OuterIterator
object(FilterIterator@anonymous)#2 (1) {
    ["search"]=> string(8) "bc_break"
}
```

As you can see, instanceof correctly reports that the anonymous class implements OuterInterface (because it extends FilterIterator, which, in turn, implements OuterInterface). You can also see that var_dump() reports the name of the anonymous class as FilterIterator@anonymous.

Now that you have an idea of how anonymous class naming has changed in PHP 8, let's have a look at changes in namespace handling.

## Understanding changes in namespaces

The concept of a namespace was introduced in PHP 5.3 as a means of isolating hierarchies of classes. Unfortunately, the original algorithm that was used to parse namespace names had several flaws. In addition to being overly complicated, the way in which namespace and class names were tokenized internally was performed in an inconsistent manner, leading to unexpected errors.

Before we get into the benefits and potential backward compatible breaks, let's have a look at how the namespace tokenization process has changed.

## Discovering differences in tokenization

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

The impact of this change produces extremely positive results. For one, you can now use reserved key words as part of a namespace. Also, in the future, as new keywords are introduced into the language, PHP will not force you to change namespaces in your applications. The new tokenization process also facilitates the use of Attributes with namespaces.

First, let's have a look at using keywords in a namespace.

## Using reserved keywords in a namespace

The namespace tokenization process in PHP 7 produced a series of string (T_STRING) and backslash (T_NS_SEPARATOR) tokens. The problem with this approach is that if one of the strings happened to be a PHP keyword, a syntax error was immediately thrown during the parsing process. PHP 8, however, produces just a single token, as shown earlier, in Table 9.5. Ultimately, this means that you can put pretty much anything in a namespace and don't have to worry about reserved keyword conflicts.

The following code example illustrates the difference in namespace handling between PHP 8 and earlier versions. In this example, a PHP keyword is being used as part of the namespace. In PHP 7, due to its inaccurate tokenization process, List is seen as a keyword rather than part of the namespace:

```
```php
// /repo/ch09/php8_namespace_reserved.php
namespace List\Works\Only\In\PHP8;
class Test {
    public const TEST = 'TEST';
}
echo Test::TEST . "\n";
```
```

Here is the output from PHP 7:

```
```
root@php8_tips_php7 [ /repo/ch09 ]#
php php8_namespace_reserved.php
PHP Parse error: syntax error, unexpected 'List' (T_LIST),
expecting '{' in /repo/ch09/php8_namespace_reserved.php on line 3
```
```

In PHP 8, the program code snippet works as expected, as shown here:

```
```
root@php8_tips_php8 [ /repo/ch09 ]#
php php8_namespace_reserved.php
TEST
```
```

Now that you have an idea of how the tokenization process differs between PHP 8 and earlier PHP versions, as well as its potential benefits, let's have a look at potential backward compatible code breaks.

## Exposing bad namespace naming practices

The same PHP 8 tokenization process that releases you from having to worry about keyword conflicts may also end up exposing bad namespace naming practices. Any namespaces with white space in them are now considered invalid. However, it's a bad practice to include whitespace in a namespace anyhow!

The following simple code example illustrates this principle. In this example, you'll notice that the namespace includes white space:

```
// /repo/ch09/php7_namespace_bad.php
namespace Doesnt \Work \In \PHP8;
class Test {
    public const TEST = 'TEST';
}
echo Test::TEST . "\n";
```

If we run this code in PHP 7, it works OK. Here's the PHP 7 output:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_namespace_bad.php
TEST
```

In PHP 8, however, a ParseError is thrown, as shown here:

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_namespace_bad.php
PHP Parse error: syntax error, unexpected fully qualified name "\Work", expecting "{" in /repo/ch09/php7_namespace_bad.php on line 3
```

The space serves as a delimiter that's used by the parser during the tokenization process. In this code example, PHP 8 assumes that the namespace is Doesnt. The next token is \ Work, which marks a fully qualified class name. However, it's not expected at this point, which is why an error was thrown.

This concludes our discussion on the changes that have been made to namespace handling in PHP 8. You are now in a better position to create namespace names in PHP 8 that not only follow best practices, but also take advantage of its independence from keyword naming conflicts.

## Summary

PHP 8, as you learned, is much stricter in terms of how it defines magic methods. In this chapter, you learned about method signature changes and how to reduce potential bugs by using magic methods properly. You also learned about the method signature changes in both the Reflection and PDO extensions. With the knowledge you've gained in this chapter, you can avoid potential problems when migrating to PHP 8. In addition, you learned about changes in how static methods can be called, as well as a new static return type.

Then, you learned how to make the best use of private methods, as well as how to exert greater control over anonymous classes. You also picked up a few tips on new syntax possibilities and which methods are now obsolete due to changes in the language.

You also learned how to correctly use interfaces and traits to facilitate efficient use of your code. You learned about the new interfaces that were introduced to bring the DOM extension up to the new DOM Living Standard. In addition, you gained insights into using new methods introduced in the DateTime extension.

Finally, you learned how to clean up namespace usage and produce tighter code. You now have a better understanding of how the namespace tokenization process was inaccurate and how it's been improved in PHP 8.

The next chapter will present you with something every developer strives for: tips, tricks, and techniques to improve performance.
