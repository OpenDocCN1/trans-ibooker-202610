critical point. A developer might not even need to think about applying a label to a report; it’s inherited from the underlying data source.

This inheritance is what makes sensitivity labels so powerful for governance. If an organization has a policy that a Confidential report can’t be exported to a PDF file or printed, Fabric will enforce that rule for all items tagged with that label. The developer’s job is to apply the correct label, and the platform will take care of the rest. This creates a very robust governance model that is hard to circumvent.

> **NOTE**
> 
> Sensitivity labels are all about classification and governance. They do not replace row-level or column-level security. A label can enforce a policy that prevents a user from exporting a report, but it will not filter the data that the user can see. The two layers of security work together: one controls who can see what, and the other controls how the data can be used.

# Endorsing Items

In an organization where everyone is creating reports, datasets, and other items, it can quickly become hard to figure out what’s what. How do you know if a specific report is the “official” one you should be using for your business decisions? How do you know it’s accurate and trustworthy? This is where endorsements come in.

*Endorsements* provide a way to signal the quality and reliability of an item in Fabric. They are simple visual cues that tell users this specific report, semantic model, or other artifact has been vetted and approved. Think of them as stamps of approval from the person or team who owns the