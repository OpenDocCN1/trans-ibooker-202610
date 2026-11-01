# Create lifecycle rule

# Lifecycle rule configuration

Lifecycle rule name
Enter rule name
Up to 255 characters

Choose a rule scope
• Limit the scope of this rule using one or more filters
• Apply to all objects in the bucket

# Filter type

You can filter objects by prefix, object tags, object size, or whatever combination suits your usecase.

Prefix
Add filter to limit the scope of this rule to a single prefix.
Enter prefix
Don't include the bucket name in the prefix. Using certain characters in key names can cause problems with some applications and protocols. Learn more

Object tags
You can limit the scope of this rule to the key/value pairs added below.
Add tag

Object size
You can limit the scope of this rule to apply to objects based on their size. For example, you can filter out objects that might not be cost effective to transition to Glacier Flexible Retrieval (formerly Glacier) because of per-object fees.
Specify minimum object size
Specify maximum object size

# Lifecycle rule actions

Choose the actions you want this rule to perform. Per-request fees apply. Learn more or see Amazon S3 pricing

+   - Move current versions of objects between storage classes
+   - Move noncurrent versions of objects between storage classes
+   - Expire current versions of objects
+   - Permanently delete noncurrent versions of objects
+   - Delete expired object delete markers or incomplete multipart uploads
+   These actions are not supported when filtering by object tags or object size.

# Review transition and expiration actions

| Current version actions | Noncurrent versions actions |
| Day 0
No actions defined. | Day 0
No actions defined. |

Figure 5.7 – Configuring an Amazon S3 lifecycle rule

Apart from Amazon S3 Lifecycle, Amazon S3 also offers Amazon S3 Intelligent-Tiering.

Amazon S3 Intelligent-Tiering is a storage class that automatically moves data to the most cost-effective storage tier, based on changing access patterns. The data is automatically transitioned between two access tiers: one for frequently