
The results of this small code example running in PHP 7 are surprising:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_trait_abstract_signature.php
333.333
```

As you can see from the results, the method signature of the abstract definition in the trait is completely ignored! In PHP 8, however, the outcome is much different. Here is the code's output running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_trait_abstract_signature.php
PHP Fatal error: Declaration of class@anonymous::add(float $a, float $b): float must be compatible with Test1::add(int $a, int $b): int in /repo/ch09/php7_trait_abstract_signature.php on line 9
```

The preceding PHP 8 output shows us that good coding practices are enforced, regardless of the source of the abstract method definition.

The last topic in this section will show you how to handle abstract private methods in traits.

## Handling private abstract methods in traits

Generally, in PHP, you can't enforce control over an abstract private method in an abstract super class as it will not be inherited. In PHP 8, however, you can define an abstract private method in a trait! This can be used as a use-case enforcement mechanism when you're doing API development where the using class is required to define a specified private method.

Please note that although you can designate an abstract method as private in a PHP 8 trait, trait method visibility can easily be overridden in the class that uses the trait. Accordingly, we will not show any code examples in this section as the effect of a private abstract trait method is exactly the same as using an abstract trait method at other visibility levels.

> **Tip**
> 
> For more information on trait abstract method handling in PHP 8, have a look at this RFC: https://wiki.php.net/rfc/abstract_trait_method_validation.

Now, let's have a look at general usage changes in private methods.

## Dealing with private methods

One of the reasons why developers create a super class is to exert a certain degree of control over the method signatures of subclasses. During the parsing phase, PHP normally confirms that the method signatures match. This leads to proper use of your code by other developers.

In the same vein, it does not make sense to have PHP perform the same rigorous method signature check if a method is marked as private. The purpose of a private method is that it is invisible to the extending class. If you define a method of the same name in the extending class, you should be free to define it at will.

To illustrate this problem, let's define a class called Cipher with a private method called encrypt(). The OpenCipher subclass redefines this method, causing a fatal error when running under PHP 7:

- First, let's define a Cipher class whose constructor generates random values for $key and $salt. It also defines a public method called encode() that makes a call to the private encrypt() method:

```php
// /repo/src/Php7/Encrypt/Cipher.php
namespace Php7\Encrypt;
class Cipher {
    public $key = '';
    public $salt = 0;
    public function __construct() {
        $this->salt = rand(1,255);
        $this->key = bin2hex(random_bytes(8));
    }
    public function encode(string $plain) {
        return $this->encrypt($plain);
    }
}
```

- Next, let's define a private method called encrypt() that produces encrypted text using str_rot13(). Note that the method is marked as final. Although this does not make any sense, for the purposes of this illustration, assume that this is intended:

```php
final private function encrypt(string $plain) {
    return base64_encode(str_rot13($plain));
}
```

- Finally, let's define a brief calling program that creates class instances and calls the methods that have been defined:

```php
// /repo/ch09/php7_oop_diffs_private_method.php
include __DIR__ . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php7\Encrypt\{Cipher,OpenCipher};
$text = 'Super secret message';
$cipher1 = new Cipher();
echo $cipher1->encode($text) . "\n";
$cipher2 = new OpenCipher();
var_dump($cipher2->encode($text));
```

If we run the calling program in PHP 7, we'll get the following output:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_oop_diffs_private_method.php
RmhjcmUgZnJwZKJnIHpyZmZudHI=
PHP Fatal error: Cannot override final method Php7\Encrypt\Cipher::encrypt() in /repo/src/Php7/Encrypt/OpenCipher.php on line 21
```

Here, you can see that the output from Cipher was correctly produced. However, a fatal Error has been thrown, along with a message stating that we are unable to override a final method. Theoretically, a private method should be completely invisible to the subclass. However, as you can clearly see from the output, this is not the case. The method signature of a private method of the Cipher superclass affects our ability to redefine the same method in the subclass.

In PHP 8, however, this paradox has been resolved. Here is the output of the same code running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_oop_diffs_private_method.php
PHP Warning: Private methods cannot be final as they are never overridden by other classes in /repo/src/Php7/Encrypt/Cipher.php on line 17
RmhjcmUgZnJwZXJnIHpyZmZudHI=
array(2) {
["tag"]=> string(24) "woD6Vi73/IXLaKHFGUC3aA=="
["cipher"]=> string(28) "+vd+jWKqo8WFPd7SakSvszkoIX0="
```

As you can see from the preceding output, the application succeeded, and output from both the parent and child classes is being displayed. We can also see a Warning, informing us that the private methods cannot be marked as final.

> **Tip**
> 
> For more information on the background discussion on private method signatures, have a look at this documentation reference:
> https://wiki.php.net/rfc/inheritance_private_methods.

Now that you have an idea of how PHP 8 prevents subclasses from seeing private methods in the superclass, let's turn our attention to differences in anonymous classes in PHP 8.

## Controlling anonymous class usage

**Anonymous classes**, by their very definition, do not have a name. However, for the purposes of information, PHP informational functions such as `var_dump()`, `var_export()`, `get_class()`, and other classes in the Reflection extension will report the anonymous class simply as `class@anonymous`. However, when an anonymous class extends another class or implements an interface, it might be of some use to have PHP informational functions reflect this fact.

In PHP 8, anonymous classes that extend a class or implement an interface now reflect that fact by changing the label that's assigned to the anonymous class to Xyz@anonymous, where Xyz is the name of the class or interface. If the anonymous class implements more than one interface, only the first interface will appear. If the anonymous class extends a class and also implements one or more interfaces, the name of the class it extends will appear in its label. The following table summarizes these possibilities:

| Context | New Name |
|---|---|
| extends Xyz | Xyz@anonymous |
| implements Abc | Abc@anonymous |
| implements Abc, Def | Abc@anonymous |
| extends Xyz implements Abc | Xyz@anonymous |

Table 9.4 – Anonymous class promotion

Bear in mind that PHP can already test the anonymous class to see if it falls under a certain line of inheritance. The instanceof operator, for example, can be used for this purpose. The following example illustrates how to test the anonymous class for inheritance, as well as how to view its new name:

- For this example, we will define a DirectoryIterator instance that grabs a list of files from the current directory:
- Then, we will define an anonymous class that extends FilterIterator. In this class, we will define the accept() method, which produces a Boolean result. If the result is TRUE, then that item will appear in the final iteration:

```php
// /repo/ch09/php8_oop_diff_anon_class_renaming.php
$iter = new DirectoryIterator(__DIR__);
```

```php
$anon = new class ($iter) extends FilterIterator {
    public $search = '';
    public function accept() {
        return str_contains(
            $this->current(), $this->search);
    }
};
```
