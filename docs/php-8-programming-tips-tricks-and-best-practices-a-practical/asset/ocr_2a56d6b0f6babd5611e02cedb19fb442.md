In PHP 8, however, because SimpleXMLElement now implements RecursiveIterator, the code to produce the same results is simpler.

3. As with the example shown earlier, we define a SimpleXMLElement instance from an external file. There is no need to define a recursive function, however— all we need to do is define a RecursiveIteratorIterator instance, as follows:

```
```php
// ///repo/ch07/php8_simple_xml.php
$fn = __DIR__ . '/includes/tree.xml';
$xml = simplexml_load_file($fn);
$iter = new RecursiveIteratorIterator($xml,
    RecursiveIteratorIterator::SELF_FIRST);
```
```

4. After that, all we need is a simple foreach() loop, with the same internal logic as in the preceding example. There's no need to check to see if a branch node exists, nor is there a need for recursion—that's taken care of by the RecursiveIteratorIterator instance! The code you'll need is illustrated here:

```
```php
foreach ($iter as $branch) {
    if (!empty($branch->descendent)) {
        echo $branch->descendent;
        echo ($branch->descendent['gender'] == 'F')
            ? ', daughter of '
            : ', son of ';
        echo $branch['name'];
        if (empty($branch->spouse)) echo "\n";
        else echo ", married to {$branch->spouse}\n";
    }
}
```
```

The output from this code example running in PHP 8 is shown here. As you can see, the output is exactly the same:

```
```
root@php8_tips_php8 [ /repo/ch07 ]# php php8_simple_xml.php
George V, son of Windsor, married to Mary of Treck
George VI, son of George V, married to Elizabeth Bowes-Lyon
Elizabeth II, daughter of George VI, married to Philip
Prince Charles, son of Elizabeth II, married to Diana Spencer
William, son of Prince Charles, married to Kate Middleton
```
```