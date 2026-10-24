| Path | HTTP Method | Description |
|---|---|---|
| /api/catalog/ GET |  | Returns a catalog for all items available in the store, matching items from Inventory service with data from Catalog service. |

You find it in this [book's GitHub repository](book's GitHub repository).

```
package com.redhat.cloudnative.catalog;

import java.util.List;
import java.util.Spliterator;
import java.util.stream.Collectors;
import java.util.stream.StreamSupport;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping(value = "/api/catalog") ①
public class CatalogController {

    @Autowired ②
    private ProductRepository repository; ③

    @ResponseBody
    @GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    public List<Product> getAll() {
        Spliterator<Product> products =
        repository.findAll().spliterator();
        return StreamSupport.stream(products,
        false).collect(Collectors.toList());
    }
}
```

① @RequestMapping indicates the above REST service defines an endpoint that is accessible via HTTP GET at /api/catalog