```
```php
strpos($this->contents, $search2) !== FALSE)
{
    $this->messages[] = sprintf(
        self::ERR_REMOVED, $func, $replace);
    $found++;
}
}
if ($found === 0)
    $this->messages[] = sprintf(
        self::OK_PASSED, __FUNCTION__);
    return $found;
}
```
```

The main problem with this approach is that if the function is not preceded by a space, its use would not be detected. However, if we do not include the leading space in the search, we could end up with a false positive. For example, without the leading space, every single instance of foreach() would trigger a warning by the break scanner when looking for each()!

2. Next we have a look at a method that scans for is_resource() usage. If a reference is located, this method runs through the list of functions that no longer produce a resource. If both is_resource() and one of these methods is located, a potential BC break is flagged:

```
```php
public function scanIsResource() : int {
    $found = 0;
    $search = 'is_resource';
    if (strpos($this->contents, $search) === FALSE)
        return 0;
    $config = $this->config[self::KEY_RESOURCE];
    foreach ($config as $func) {
        if ((strpos($this->contents, $func) !== FALSE)) {
            $this->messages[] =
                sprintf(self::ERR_IS_RESOURCE, $func);
            $found++;
        }
    }
    if ($found === 0)
        $this->messages[] =
```
```