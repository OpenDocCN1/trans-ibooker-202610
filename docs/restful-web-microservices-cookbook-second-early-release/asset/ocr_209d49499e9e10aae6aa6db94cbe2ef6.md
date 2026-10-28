include Collection+JSON, SIREN, and UBER. HAL plus HAL-FORMS is also a good choice. These formats are not the only possible choices but they are the ones that contain the most hypermedia features based on the Hypermedia Factors 16 measure.

> WARNING
> 
> Note that HAL alone only supports describing safe, idempotent actions (links) and does not support including details of HTTP methods and properties for forms. That is why I recommend using the HAL-FORMS extension in addition to the original HAL format when creating APIs based on the HAL representation format.

When you emit hypermedia formats, you have the ability to reflect new actions (added forms) as well as safe updates to those actions (added properties, updated URLs, and changed HTTP methods). These are the most common things that will change in a service interface over time.

There are still limitations to this approach. Using hypermedia formats allows you to make it easy to change the addresses (URLs), objects (data properties), and actions (forms) safely and consistently. However, you may still need to modify the domain properties (names of things) over time, too. You might, for example, need to add a new property to the list of possible inputs (middleName) or a new output value (alternateEmail) or a new request parameter (/filter/?newRegionFilter=south). In this case, you’ll need to create (and document using ALPS or some other format) a new vocabulary document and allow clients to discover and select these additional vocabularies at runtime (see Chapter 2 for details). There are some recipes in this chapter that address this ability to select vocabularies, too.

# From Self-Servicing to Find and Bind

Another important element of services on the web is the ability for API consumers to “self-service” their onboarding experience. The power to simply select the desired service and instantly use it is a worthy goal. That’s