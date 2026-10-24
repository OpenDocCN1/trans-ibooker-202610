+   - Data Transformation (image, video)
+   - PDF generation
+   - Single-page apps for mobile devices
+   - Chatbots

With this approach, you can focus on convenience, as it is generally offered as *best-effort*. Failures are tolerated and short actions are preferred. That’s why the serverless model is not good for use cases such as real-time applications, long-running tasks, contexts where reliability and/or atomicity are key. It’s up to the developer to take care of verifying that inputs and outputs have been successfully processed by any serverless function involved. This gives great flexibility and high scalability to at least some part of the overall architecture.

# Use cases: Data, AI and Machine Learning

The Serverless model helps avoid common headaches for capacity planning for projects, as it mitigates over-provisioning and under-provisioning, thereby reducing IT cost of idle resources. With Serverless, all of the resources consumed are tailored to the actual usage as the applications start only when invoked, and there’s no need to pre-allocate or *measure-and-update* hardware resources.

This is very important when you have to analyze, in real-time, a large amount of data, and that’s why Serverless is gaining lots of attention from data scientists and ML experts, as the functions that can process data for analysis are flexible and leave a minimal footprint. On the other hand, Serverless doesn’t pair well with all of the design principles of existing ML frameworks. A certain amount of tolerance is also required, in particular for processes that may take longer such as model training.

If you look at Figure 7-3, we see an example of a Serverless-driven architecture for Machine Learning for classification and prediction. The process starts with a trigger to get an inference of a group of objects from a