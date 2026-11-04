```
```php
} elseif (!strpos($name, $searchPath)) {
    $discard[] = $name;
} elseif ($obj->getExtension() != $searchExt) {
    $non_match[] = $name;
} else {
    $matching[] = $name;
}
}
show($matching, $non_match);
}
```
```

3. Instead of using if / elseif / else, however, some developers might be tempted to refactor this function using nested ternary operators instead. Here is how the same code used in the preceding step might look:

```
```php
function find_using_tern($iter, $searchPath,
    $searchExt) {
    $matching = [];
    $non_match = [];
    $discard = [];
    foreach ($iter as $name => $obj) {
        $match = !$obj->isFile()
            ? $discard[] = $name
            : !strpos($name, $searchPath)
                ? $discard[] = $name
                : $obj->getExtension() != $searchExt
                    ? $non_match[] = $name
                    : $matching[] = $name;
    }
    show($matching, $non_match);
}
```
```

The output from both functions produces identical results in PHP 7, as illustrated in the following screenshot: