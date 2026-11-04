2. We then use the fgetcsv() function to pull a row of data into $line, and pack latitude and longitude information into a $data array. Note in the following code snippet that we filter out rows of data on cities with a population less than $target (in this case, less than 10 million):

```
while ($line = fgetcsv($fh, '', "\t")) {
    $popNum = $line[14] ?? 0;
    if ($popNum > $target) {
        $city = $line[1] ?? 'Unknown';
        $data[$city] = $line[4] . ',' . $line[5];
    }
}
```

3. We then close the file handle and sort the array by city name. To present the output, we use each() to walk through the array, producing key/value pairs, where the city is the key, and latitude and longitude is the value. The code is illustrated in the following snippet:

```
fclose($fh);
ksort($data);
printf($pattern, 'City', 'Latitude/Longitude');
printf($pattern, '----', '------------------');
while ([$city, $latLon] = each($data)) {
    $city = str_pad($city, 30, ' ', STR_PAD_LEFT);
    printf($pattern, $city, $latLon);
}
```

Here is the output as it appears in PHP 7:

```
root@php8_tips_php7 [/repo/ch08]# php php7_each.php
                City :   Latitude/Longitude
                ---- :   ------------------
            Beijing :    39.9075,116.39723
        Buenos Aires :   -34.61315,-58.37723
              Delhi :    28.65195,77.23149
              Dhaka :    23.7104,90.40744
          Guangzhou :    23.11667,113.25
           Istanbul :    41.01384,28.94966
            Karachi :    24.8608,67.0104
```