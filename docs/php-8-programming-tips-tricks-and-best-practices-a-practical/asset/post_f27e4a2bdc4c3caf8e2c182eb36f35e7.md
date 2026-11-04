
7. The following output demonstrates that the PHP echo command now produces a LF at the end of each command:

| Revised echo command adds LF: |
|---|
| A |
| B |
| C |

It's also important to note that modifying the PHP library C-language zend_write function has an impact on all PHP native functions using this C-language function. This includes print(), printf() (and its variants), and so forth.

This concludes our discussion of using the FFI extension in a PHP application. You now know how to incorporate native C functions from an external shared library. You also know how to substitute a PHP callback for a native PHP core or extension shared library, giving you the potential to alter the behavior of the PHP language itself.

## Summary

In this chapter, you learned about the FFI, its history, and how it can be used to facilitate rapid PHP extension prototyping. You also learned that although the FFI extension should not be used to improve speed, it also serves the purpose of allowing your PHP application to directly call native C functions from an external C library. The power of this ability was demonstrated through an example that called a bubble-sort function from an external C library. This same capability can be extended to encompass any of the thousands of C libraries available, including machine learning, optical character recognition, communications, encryption; ad infinitum.

In this chapter, you acquired a deeper understanding of how PHP itself operates at the C-language level. You learned how to create and directly use C-language data structures, giving you the ability to interact, and even override, the PHP language itself. In addition, you now have an idea how to incorporate the functionality of any C-language library directly into a PHP application. A further benefit of this knowledge is that it serves to enhance your career prospects if you find a job with a company that either plans to develop, or has already developed, its own custom PHP extension.

The next chapter marks the beginning of a new section of the book, PHP 8 Tricks. In the next section, you will learn about backward-compatibility issues when upgrading to PHP 8. The next chapter specifically addresses backward-compatibility issues with respect to OOP.

## Section 2: PHP 8 Tricks

In this part, you are taken into the dark corners of PHP 8: the places where backward-compatibility breaks exist. This part guides you through the critical process of migrating an existing application to PHP 8.

The following chapters are included in this section:

- Chapter 5, Discovering Potential OOP Backward-Compatibility Breaks
- Chapter 6, Understanding PHP 8 Functional Differences
- Chapter 7, Avoiding Traps When Using PHP 8 Extensions
- Chapter 8, Learning about PHP 8's Deprecated or Removed Functionality

## 5 Discovering Potential OOP Backward-Compatibility Breaks

This chapter marks the beginning of Part 2 of the book, *PHP 8 Tricks*. In this part, you'll discover the dark corners of PHP 8: the place where **backward-compatibility breaks** exist. This part gives you insight into how to avoid problems before migrating an existing application to PHP 8. You will learn what to look for in your existing code that could cause it to stop working after a PHP 8 upgrade. Once you master the topics presented in this part of the book, you will be well equipped to modify existing code in such a manner that it continues to function normally following a PHP 8 upgrade.

In this chapter, you will be introduced to new PHP 8 features specific to **object-oriented programming (OOP)**. The chapter provides you with plenty of short code examples that clearly illustrate the new features and concepts. This chapter is critical in helping you quickly take advantage of the power of PHP 8 as you adapt the code examples for your own practice. The focus of this chapter is on situations where object-oriented code might break after a PHP 8 migration.

Topics covered in this chapter include the following:

- Discovering core OOP coding differences
- Navigating changes in magic methods
- Taking control of serialization
- Understanding expanded PHP 8 variance support
- Handling **Standard PHP Library (SPL)** changes

## Technical requirements

To examine and run the code examples provided in this chapter, the minimum recommended hardware is the following:

- x86_64 based desktop PC or laptop
- 1 gigabyte (GB) of free disk space
- 4 GB of RAM
- A 500 kilobits per second (Kbps) or faster internet connection

In addition, you will need to install the following software:

- Docker
- Docker Compose

Please refer to the *Technical requirements* section of *Chapter 1, Introducing New PHP 8 OOP Features*, for more information on Docker and Docker Compose installation, as well as how to build the Docker container used to demonstrate code explained in this book. In this book, we refer to the directory in which you restored the sample code for this book as `/repo`.

The source code for this chapter is located here: https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices.

We can now begin our discussion by examining core OOP coding differences.

## Discovering core OOP coding differences

There are a number of significant changes to how you are able to write OOP code in PHP 8. In this section, we focus on three key areas that might present you with potential backward-compatibility breaks. The areas we address in this section are common bad practices associated with making static method calls, handling object properties, and PHP autoloading.

After reading this section, and working your way through the examples, you are in a better position to spot OOP bad practices and to learn how PHP 8 has placed restrictions on such usage. In this chapter, you learn good coding practices, which will ultimately make you a better programmer. You will also be able to address changes in PHP autoloading that can potentially cause failure in an application migrated to PHP 8.

Let's first look at how PHP 8 has tightened up on making static calls.

### Handling static calls in PHP 8

Surprisingly, PHP versions 7 and below allowed developers to make a static call to a class method not declared static. At first glance, any future developer reviewing your code immediately assumes that the method has been defined as static. This can lead to unexpected behavior as the future developer, operating under a false assumption, starts to misuse your code.

In this simple example, we define a Test class with a nonStatic() method. In the procedural code that follows the class definition, we echo the return value of this method, however, in doing so we make a static call:

```
// /repo/ch05/php8_oop_diff_static.php
class Test {
    public function notStatic() {
        return __CLASS__ . PHP_EOL;
    }
}
echo Test::notStatic();
```

When we run this code in PHP 7, here is the result:

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_oop_diff_static.php
```

```
PHP Deprecated:  Non-static method Test::notStatic() should not be called statically in /repo/ch05/php8_oop_diff_static.php on line 11
Test
```

As you can see from the output, PHP 7 issues a deprecation notice, but allows the call to be made! In PHP 8, however, the result is a fatal Error, as shown here:

```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_oop_diff_static.php
PHP Fatal error:  Uncaught Error: Non-static method
Test::notStatic() cannot be called statically in /repo/ch05/
php8_oop_diff_static.php:11
```

Calling a non-static method using static method call syntax is a bad practice in the sense that well-written code makes the intention of the code developer crystal clear. If you do not define a method as static, but later call it in a static sense, a developer assigned to maintain your code in the future might become confused and could make wrong assumptions about the original intent of the code. The end result will be even more bad code!

In PHP 8, you can no longer call a non-static method using a static method call. Let's now have a look at another bad practice involving treating object properties as keys.

### Dealing with object property handling changes

Arrays have been a central feature in PHP all the way back to the earliest versions. OOP, on the other hand, was not introduced until PHP 4. In the early days of OOP, array functions were often expanded to accommodate object properties. This led to a blurring of the distinction between an object and an array, which in turn spawned a number of bad practices.

In order to maintain a clear separation between array handling and object handling, PHP 8 now restricts the array_key_exists() function to only accept an array as an argument. To illustrate this, consider the following example:

- 1. First, we define a simple anonymous class with a single property:

```
// /repo/ch05/php8_oop_diff_array_key_exists.php
$obj = new class () { public $var = 'OK.'; };
```
