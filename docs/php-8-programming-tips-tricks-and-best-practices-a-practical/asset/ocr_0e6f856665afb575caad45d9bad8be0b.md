```
$fmt = 'Y-m-d';
$aging = [];
```

2. Now, let's define a loop that adds the intervals to a DateTime instance to produce (hopefully!) an array representing days 0, 30, 60, and 90. Veteran developers have most likely already spotted the problem!

```
$dti = new DateTime('now');
foreach ($days as $span) {
    $interval = new DateInterval('P' . $span . 'D');
    $item = $dti->add($interval);
    $aging[$span] = clone $item;
}
```

3. Next, display the set of dates that have been produced:

```
echo "Day\tDate\n";
foreach ($aging as $key => $obj)
    echo "$key\t" . $obj->format($fmt) . "\n";
```

4. The output, which is a complete disaster, is shown here:

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_date_time_30-60-90.php
Day    Date
0      2021-11-20
30     2021-06-23
60     2021-08-22
90     2021-11-20
```

As you can see, the problem is that the DateTime class is not immutable. Thus, every time we add a DateInterval, the original value is altered, resulting in the inaccurate dates shown.

5. By making one simple alteration, however, we can correct this problem. Instead of originally creating a DateTime instance, all we need to do is create a DateTimeImmutable instance instead:

```
$dti = new DateTimeImmutable('now');
```