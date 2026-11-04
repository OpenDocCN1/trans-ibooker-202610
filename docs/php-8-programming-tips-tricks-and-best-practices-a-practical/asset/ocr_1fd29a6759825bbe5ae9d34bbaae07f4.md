2. Recall that the C language treats character data as an array, so we can't just directly assign a string, even if using the cdata property. Accordingly, we need to define an anonymous function that populates the instances with letters of the alphabet. We use the following code to do this:

```
```
$populate = function ($cdata, $start, $offset, $num) {
    for ($x = 0; $x < $num; $x++)
        $cdata[$x + $offset] = chr($x + $offset +
                                    $start);
    return $cdata;
};
```
```

3. Next, we use the function to populate the four FFI\CData instances with differing sets of letters, as follows:

```
```
$a = $populate($a, 65, 0, 6);
$b = $populate($b, 65, 0, 3);
$b = $populate($b, 85, 3, 3);
$c = $populate($c, 71, 0, 6);
$d = $populate($d, 71, 0, 6);
```
```

4. We can now use the FFI::string() method to display the contents thus far, as follows:

```
```
$patt = "%2s : %6s\n";
printf($patt, '$a', FFI::string($a, 6));
printf($patt, '$b', FFI::string($b, 6));
printf($patt, '$c', FFI::string($c, 6));
printf($patt, '$d', FFI::string($d, 6));
```
```

5. Here is the output from the printf() statements:

```
```
$a : ABCDEF
$b : ABCXYZ
$c : GHIJKL
$d : GHIJKL
```
```