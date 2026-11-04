```
if (empty($node->spouse)) echo "\n";
else echo ", married to {$node->spouse}\n";
if (!empty($node->branch))
    recurse($node->branch);
}
}
```

+   2. We then create a SimpleXMLElement instance from the external XML file and call the recursive function, as follows:

```
// ///repo/ch07/php7_simple_xml.php
$fn = __DIR__ . './includes/tree.xml';
$xml = simplexml_load_file($fn);
recurse($xml);
```

This code block works in both PHP 7 and PHP 8. Here is the output running in PHP 7:

```
root@php8_tips_php7 [ /repo/ch07 ]#
php php7_simple_xml.php
George V, son of Windsor, married to Mary of Treck
George VI, son of George V, married to Elizabeth Bowes-Lyon
Elizabeth II, daughter of George VI, married to Philip
Prince Charles, son of Elizabeth II, married to Diana Spencer
William, son of Prince Charles, married to Kate Middleton
Harry, son of Prince Charles, married to Meghan Markle
Princess Anne, daughter of Elizabeth II, married to M.Phillips
Princess Margaret, daughter of George VI, married to A.Jones
Edward VIII, son of George V, married to Wallis Simpson
Princess Mary, daughter of George V, married to H.Lascelles
Prince Henry, son of George V, married to Lady Alice Montegu
Prince George, son of George V, married to Princess Marina
Prince John, son of George V
```