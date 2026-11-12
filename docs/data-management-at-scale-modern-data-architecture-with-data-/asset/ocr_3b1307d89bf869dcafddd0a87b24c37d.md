# Automate as much as possible.

Try to make the process of data onboarding and consumption as easy and automated as possible. At first, processes may be manual and metadata might sit in Microsoft Forms or Excel. But as you progress, try to automate whatever is possible.

# Standardize authorization and data access.

Align your security practices with your data management workflow across the enterprise to make data widely available and to simplify the process of data democratization.

# Select the right tools.

Designing a security architecture is difficult, so consider using out-of-the-box services with established (proven) patterns.

# Implement guidance from the start.

If good guidance for managing data products is not provided, it will be harder to allow fine-grained data access.

Regarding that last point, it’s important to consider how you will handle ownership and manage data products when business capabilities are shared. What guidance do you need to provide to your teams? We’ve already discussed this in “Shared capabilities” on page 43, but there are different tricks you can apply here. One method, as seen at the top of Figure 8-8, is to physically split out data by creating multiple data products for different owners. This approach usually works best when data ownership and usage is coarse-grained—that is, when the data is linked to only a few owners, and consumers who request data typically request all the data that corresponds to one owner.

Another method, as seen at the bottom of Figure 8-8, is to encapsulate metadata for security during the data product creation process. This metadata is used for security and custom presentations, which typically require selections (dynamic virtual views) of the data. For example, when a consumer requests a data product, a WHERE clause is set for filtering out data that the consumer isn’t allowed to see or use.