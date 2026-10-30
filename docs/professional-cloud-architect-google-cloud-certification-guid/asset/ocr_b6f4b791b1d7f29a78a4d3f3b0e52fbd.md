There are two types of holds and an object can have one, both, or neither hold applied. When an object is stored in a bucket without a retention policy, both hold types behave in the same way. However, if a retention policy has been applied to the bucket, the holds have different effects on an object when the hold is released:

+   - **Event-based hold**: This resets the object's time in the bucket for the retention period.
+   - **Temporary hold**: This does not affect the object's time in the bucket for the retention period.

Let's look at an example to make things a little clearer. Let's say we have two objects – Object A and Object B – that we add to a storage bucket that has a 1-month retention period. We apply an **event-based hold** on Object A and a **temporary hold** on Object B. Due to the retention period alone, we would usually be able to delete both objects after 1 month. However, because we applied an **object hold** to each object, we are unable to do so. If we release the hold on Object A, then due to the **event-based hold**, the object must remain in the bucket for another 1 month before we can delete it. If we release the hold on Object B, we can immediately delete this due to the **temporary hold** not affecting the object's time in the bucket.

# Transferring data

There will be cases where we want to transfer files to a bucket from on-premises or another cloud-based storage, and Google offers many ways to do so. There are some considerations that we should bear in mind to make sure that we select the right option. The size of the data and the available bandwidth will be the deciding factors. Let's take a quick look at a chart to show how long it would take to transfer some set data sizes over a specific bandwidth: