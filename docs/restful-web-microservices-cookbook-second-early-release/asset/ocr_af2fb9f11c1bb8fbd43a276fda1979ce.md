During the implementation phase of a service API, it is a good practice to document the rules you are using to convert internal models to external messages. This will help share the algorithms with other developers and make it easier to debug any errors you find along the way. It is not, however, a good idea to publish this representation description to API consumers. They do not need to know the internal data or object models behind your interfaces.

As time goes on, you are likely to encounter cases where the internal models of your service change. In many cases these changes should **not** result in changes to your external messages. For example, when the internal model property `user.id` is changed to `user.identifier`, there is no need to change the external property.

However, some internal model changes include new properties (`user.hatsize`) and these new properties may be reflected in the external messages when those changes will not result in breaking existing API consumers. Adding a field at the end of a `text/csv` row might be fine, but re-arranging the field order in the rows (e.g. inserting the new field as the first field in the row) is likely a bad idea.

> **TIP**
> 
> If you anticipate the internal models will change frequently, be sure to select a representation format that is designed to reduce the impact of such changes. For example, Collection+JSON is good for varying models (all properties are represented as `data.name` and `data.value`) but SIREN and HAL (since they bind to object properties directly) are not.

While you can ‘hard code’ code your service interface to **always emit** the same format (HAL, SIREN, etc.), it is a better idea to honor the HTTP content negotiation process to allow API consumers to indicate their format preferences at runtime using the `Accept` request header. See Recipe 4.5 for details on how to handle a client’s media type preferences.

It is a good idea to support more than one representation format in your service API. I typically support at least two and try to support three or