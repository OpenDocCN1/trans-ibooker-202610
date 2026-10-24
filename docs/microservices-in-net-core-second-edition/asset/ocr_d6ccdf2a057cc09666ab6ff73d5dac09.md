+   - Charge a credit card via a card reader attached to the client.
+   - Register a cash payment.
+   - Accept coupons.
+   - Print a receipt.
+   - Send an electronic receipt to the customer.
+   - Search in the product catalog.
+   - Scan one or more products to show prices and special offers related to the products.

These functions are things the system does for the cashier, but they don’t directly match the business capabilities that drive the POS system.

# IDENTIFYING BUSINESS CAPABILITIES IN THE POINT-OF-SALE DOMAIN

To identify the business capabilities that drive the POS system, you need to look beyond the list of functions. You must determine what needs to go on behind the scenes to support the functionality.

Starting with the “Search in the product catalog” function, an obvious business capability is maintaining a product catalog. This is the first candidate for a business capability that could be the scope of a microservice. Such a product catalog microservice would be responsible for providing access to the current product catalog. The product catalog needs to be updated every so often, but the chain of stores uses another system to handle that functionality. The product catalog microservice would need to reflect the changes made in that other system, so the scope of the product catalog microservice would include receiving updates to the product catalog.

The next business capability you might identify is applying special offers to invoices. Special offers give the customer a discounted price when they buy a bundle of products. A bundle may consist of a certain number of the same product at a discounted price (for example, three for the price of two), or it may be a combination of different products (say, buy A and get 10% off B). In either case, the invoice the cashier gets from the POS GUI client must take any applicable special offers into account automatically. This business capability is the second candidate to be the scope for a microservice. A special offers microservice would be responsible for deciding when a special offer applies and what the discount for the customer should be.

Looking over the list of functionality again, notice that the system should allow cashiers to “scan one or more products to show prices and special offers related to the products.” This indicates that there’s more to the special offers business capability than just applying special offers to invoices; it also includes the ability to look up special offers based on products.

If you continued the hunt for business capabilities in the POS system, you might end up with this list:

+   - Product catalog
+   - Price catalog
+   - Price calculation