# Discovering what has been removed from the core

In this section, we consider not only functions and classes that have been removed from PHP 8, but we will also have a look at usage that has been removed as well. We will then have a look at class methods and functions that still exist but no longer serve any useful purpose due to other changes in PHP 8. Knowing which functions have been removed is extremely important in order to protect against a potential code break following a PHP 8 migration.

Let's start by examining functions removed in PHP 8.

# Examining functions removed in PHP 8

There are a number of functions in the PHP language that have only been retained thus far in order to maintain backward compatibility. However, maintenance of such functions drains resources away from core language development. Further, for the most part, such functions have been superseded by better programming constructs. Accordingly, there has been a slow process whereby such commands have been slowly dropped from the language as evidence has mounted that they are no longer being used.

> **Tip**
> The PHP core team occasionally runs statistical analysis on PHP repositories based on GitHub. In this way, they are able to determine the frequency of usage of the various commands in the PHP core.