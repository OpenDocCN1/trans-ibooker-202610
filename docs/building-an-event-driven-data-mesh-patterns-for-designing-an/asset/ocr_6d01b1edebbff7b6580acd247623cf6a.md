Let's consider an extension of our ecommerce example. EcomItem.inventory_id specifies a relationship with the Inventory data (Table 8-4), indicating how many of the item are in stock.

Every time the inventory changes, say due to a sale, a return, or a received shipment, the inventory domain can emit a new state event with the updated inventory. For products that change extremely frequently (think a Black Friday “door crasher” sale), you could end up with a veritable barrage of inventory events. While it remains a valid option to publish only a sample of the events, you lose accuracy about exactly what happened while increasing the latency for an event occurring and a consumer’s ability to react to it.

If we join the EcomItem with Inventory (EcomItem.inventory_id = Inventory.id), then every single Inventory update will trigger a corresponding join on the materialized EcomItem table. This can be quite a lot of events, and consumers that don't care about the inventory quantity will be spammed with inconsequential updates.

Similarly, consider a Reviews aggregate data product that contains the top 100 reviews for a given EcomItem. While this data may not change frequently, joining it with EcomItem means that every single time we update either Reviews or EcomItem, we also re-emit the entire payload containing the top 100 reviews. Just like with fre-quent updates, many consumers may not care at all about the contents but still need to contend with the very large data size coming through the stream.

Finally, you may discover that you have both data with a very high rate of change from one stream and data with a very large payload size from another stream. Joining the two means that you’ll now have _a lot_ of data with a high rate of change. This can cause a compounding high load on your event broker, your eventification process, and your consumers.

You’ll need to find your own appropriate balance for your data products by talking to your consumers and working with your federated governance team to come up with some guidelines for helping your peers make similar decisions.

As a general rule, avoid joining in data that changes frequently or is a very large size. You may be better off leaving those components out of the enriched event and letting your consumers choose to integrate it into their own service. Of course, you can always create a new data product that includes Inventory as part of EcomItem.

# Slowly Changing Dimensions

Let’s continue with our ecommerce example. We saw that there are many EcomItems for a given Merchant, and if we want to denormalize the EcomItem.merchantId data, we’ll have to join against Merchant. However, Merchant data may not change for a very long time, though it certainly _could_ be changed at any moment.