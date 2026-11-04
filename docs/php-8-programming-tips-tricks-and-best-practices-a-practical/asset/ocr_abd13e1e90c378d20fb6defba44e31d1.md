7. We then define a CurlStrategy instance and send the same request, as illustrated in the following code snippet:

```
$curl = new CurlStrategy(curl_init());
$response = $curl->send($request);
echo $response;
```

The output from both strategies is identical. Partial output is shown here (note that this example can only be used in PHP 8!):

```
root@php8_tips_php8 [ /repo/ch07 ]#
php php8_objs_returned.php
CurlStrategy Results:
{"data":[{"id":"1227826","country":"US","postcode":"14487","city":"Livonia","state_prov_name":"New York","state_prov_code":"NY","locality_name":"Livingston","locality_code":"051","region_name":"","region_code":"","latitude":"42.8135","longitude":"-77.6635","accuracy":"4"},{"id":"1227827","country":"US","postcode":"14488","city":"Livonia Center","state_prov_name":"New York","state_prov_code":"NY","locality_name":"Livingston","locality_code":"051","region_name":"","region_code":"","latitude":"42.8215","longitude":"-77.6386","accuracy":"4"}]}
```

Let’s now have a look at another aspect of resource-to-object migration: its effect on iteration.

# Traversable to IteratorAggregate migration

The Traversable interface was first introduced in PHP 5. It has no methods and was mainly created to allow objects to iterate using a simple foreach() loop. As PHP development continues to evolve, a need often arises to obtain the inner iterator. Accordingly, in PHP 8, many classes that formerly implemented Traversable now implement IteratorAggregate instead.