You can edit Cloud Storage buckets to move between these storage classes. Note that there are additional storage classes that cannot be set using Cloud Console:

+   - **Multi-region storage**: This is equivalent to standard storage but can only be used for objects stored in multi-regions or dual regions.
+   - **Regional storage**: This is equivalent to standard storage but can only be used for objects stored in regions.
+   - **Durable Reduced Availability (DRA) storage**: This can be compared to standard storage but the DRA storage has a higher pricing for operations and a lower performance, especially in terms of availability, where it has an SLA of 99%.

Buckets get a default storage class upon their creation, and any objects inside it will inherit this class unless otherwise specified. We can change the default storage class of our bucket, but it will only apply to new objects after the change; the existing objects will remain in the original class. We can also effectively move data between buckets by using the Cloud Storage Transfer Service. However, note that moving data between locations incurs network usage costs. Additionally, we should be aware of extra costs that may be applicable. As Nearline, Coldline, and Archive storage are used to store infrequently accessed data, Google may apply retrieval or early deletion fees. A retrieval cost will be applied when we copy, read, or rewrite data or metadata that is stored in one of these storage classes.

# Data consistency

Cloud Storage will provide both strong consistency and eventual consistency in certain circumstances.

Strong consistency can be expected in the following circumstances:

+   - When uploading data to Cloud Storage, meaning the data will always be available to read after a write or update operation.
+   - Once we delete the data successfully, the files will be instantly unavailable.

Eventual consistency can be expected in the following circumstance:

+   - When access to resources is revoked. It will typically take around 1 minute for access to be revoked, though in some cases, this may take even longer.