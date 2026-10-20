# Applying Sensitivity Labels to Items

While row, column, and object security are all about controlling who can see what data, there’s a whole other layer of security that deals with the data itself, no matter where it is. This is where *sensitivity labels* come into the picture. Think of sensitivity labels as a way to classify your data and apply a set of rules and policies to it.

The concept is simple: you label data based on how sensitive it is, and then, the label enforces protection settings. It’s about data governance and ensuring that sensitive information is treated correctly throughout its entire lifecycle. This isn’t just a Fabric-only thing; these labels are part of a broader platform called Microsoft Purview Information Protection, which means the labels you use in Fabric are the same ones your organization uses in SharePoint, Teams, and Power BI. The goal is to have a consistent and unified approach to data classification.

# The Different Sensitivity Labels

The labels you can apply aren’t on a fixed list from Microsoft. They are defined by your organization’s governance team, and they reflect your company’s specific needs for data classification. A typical set of labels might look something like this:

*Public*

This is for data that can be shared freely, such as a public-facing report or a dataset that has no sensitive information.

*General*

This is the default label for most internal data. It’s for everyday business data that isn’t highly sensitive but