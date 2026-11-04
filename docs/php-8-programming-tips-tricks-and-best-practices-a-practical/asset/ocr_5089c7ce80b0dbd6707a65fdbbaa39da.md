When this code is executed, you see the currency symbols for each of the **International Organization for Standardization (ISO)** currency codes in the $test array. The same result as that shown in the preceding code snippet can be obtained in PHP 8, using the following code:

```
```php
// /repo/ch01/php8_switch.php
function get_symbol($iso) {
    return match ($iso) {
        'EGP', 'GBP' => '£',
        'CNY'        => '¥',
        'EUR'        => '€',
        'THB'        => '฿',
        default      => '$'
    };
}
$test = ['CNY', 'EGP', 'EUR', 'GBP', 'THB', 'MXD'];
foreach ($test as $iso)
    echo 'The currency symbol for ' . $iso
        . ' is ' . get_symbol($iso) . "\n";
```
```

Both examples produce an identical output, as illustrated here:

```
```
The currency symbol for CNY is ¥
The currency symbol for EGP is £
The currency symbol for EUR is €
The currency symbol for GBP is £
The currency symbol for THB is ฿
The currency symbol for MXD is $
```
```

As mentioned previously, both code examples produce a list of currency symbols for the list of ISO currency codes stored in the $test array.