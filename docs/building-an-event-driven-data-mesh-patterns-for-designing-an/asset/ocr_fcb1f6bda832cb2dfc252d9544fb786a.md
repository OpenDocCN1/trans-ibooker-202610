```
package myorg.common;

message base_user {
  String uuid = 1;
}
```

and base_item, containing a unique ID of a specific ecommerce product:

```
package myorg.common;

message base_item {
  int64 id = 1;
}
```

The data product owner can import both of these base definitions into the user_clicked_on_item schema definition, along with a common timestamp field, to compose their event definition:

```
import "common_schemas/base_user.proto"
import "common_schemas/base_item.proto"
import "google/protobuf/timestamp.proto";

message user_clicked_on_item {
  myorg.common.base_user user = 1;
  myorg.common.base_item item = 2;
  google.protobuf.Timestamp timestamp = 3;
  String websiteURI = 4;
}
```

Composing schemas using standardized forms makes it much easier to combine data products from across the organization. For example, this event can be aggregated with other events by base_user, base_item, or Timestamp, or even a combination of these three. Furthermore, adhering to common definitions means that you can also leverage common unit tests and validation plug-ins to significantly reduce the chance of making mistakes, keeping data quality and reliability high.

Common schema components should remain fairly minimal and only include fields that are mandatory for the data product owner to populate. Keep in mind that you can always create additional components that have more extensive field listings. For example, you may choose to create a common item_details component containing all of the standard information to extend base_item, and include it as an additional component in your data product's schema.

![](bbox=[0.169, 0.76, 0.25, 0.85])

Store the common schema components in a centralized repository and pull them into your local system at compile time. Store your application's schemas alongside the code in its own repository. You can then choose to publish your event schema to the data product discovery platform upon promoting it to a data product.