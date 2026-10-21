

and nonstructured data sources, such as databases, documents, web services, and electronic data interchanges. While it is preferable to ingest raw data in as close to its original form as possible, actual decisions are a function of cost, storage, and organizational maturity. This is especially true of applications in genomics, signal data, and finance, where ingesting data in raw form may quickly overwhelm processing capacity. When data is subsequently analyzed and transformed further down the pipeline, it’s important to know what the original data looked like to better understand a model’s results.

There are two data ingestion designs to choose from: batch and streaming. While this is a decidedly technical issue, the key factor in choosing between the two types is “data freshness,” or latency, and whether there is a need to process the most recent data available in near real time.

A *batch architecture* is designed to periodically ingest discrete batches of data and then push them through a series of data action components as part of a one-time processing step. These results are then available for use in your application or ML layer. Alternatively, it can serve as the basis for another data flow pipeline.

Using a batch architecture is the simplest and most common approach to ingesting large amounts of data and is sufficient for most use cases where quantity is more important than freshness and timely delivery of data. Batch architectures are also better suited for training data models, which tend to be compute intensive and rely on a training dataset that is relatively stable without much variation in the short term. As an example, batch data ingestion is useful in training a tumor detection model where a large volume of annotated medical imaging data is fed through a complex algorithm to detect tumor patterns.

A *streaming architecture*, on the other hand, is designed to ingest a live data stream as it occurs. Unlike batch architectures, streaming architectures are primarily focused on providing near real-time updates about the data being streamed in real time through the system (e.g., notifying a user that their bank transaction has posted). Streaming architectures are also more common in inference where operational data is fed through trained models for insights. Streaming architectures are useful in cases where the most recent data needs to be analyzed and acted upon in a timely manner. As an example, streaming is useful in cybersecurity applications due to the need for quick identification of a threat, minimizing the damage that can be caused in a short amount of time.

It should be noted that it is common for an organization to utilize a batch architecture in training AI models while adopting a streaming architecture for operationalizing trained models. Hybrid approaches, such as Lambda architectures, incorporate both batch and stream processing methods into a single deployment. A Kappa architecture pattern, on the other hand, seeks to eliminate the batch layer altogether to reduce complexity. In a Kappa architecture, all data is processed through stream processing methods.⁸

## Transformation

To prepare and deliver ingested data to AI applications, it needs to go through varying degrees of preparation to arrive at the application’s target format. Data transformation has additional value outside of just getting data into a preferred format for executing AI. Transforms can also be used to adapt data into formats accepted by a wide variety of other data capabilities with minimal effort. This minimizes extra work to recreate or transform data manually as your needs change.

To standardize and simplify transformation steps, it is useful to classify them into two types of transforms: static and dynamic.

Static transforms take an input and always produce the same output. These are often the most straightforward to implement and apply. Static transforms are especially useful when custom or one-off logic needs to be applied to inbound data. For instance, when migrating to a new data structure, it may be useful to add logic to transform information from the old to the new format.

Dynamic transforms, on the other hand, add flexibility to data transforms to account for potential changes in needs along the way. In this situation, a generic data transform option is used to align inputs with the desired output types. The dynamic transform then applies the appropriate transform. Multiple inputs can be fed into a single data transform to produce consistent output, simplifying the overarching data flow.

If teams subsequently encounter an issue, the dynamic transform approach can make corrections easier by allowing a configuration update, rather than a full release, to resolve the issue. Highly configurable, dynamic transforms can be widely applied and reused easily. They can also help discover a range of other potential data actions that can be reached by describing the potential output types for a given input type.

*AI-specific preprocessing*: Beyond standard data transformations, real-world applications of AI methods often involve overcoming significant challenges, such as limited data availability, to build and train models. Several AI-specific techniques are used to address some of these challenges. Examples of AI-specific preprocessing methods include data augmentation, which is an umbrella term for a suite of techniques to enhance the size and quality of training datasets so they can be used to build better models.⁹ Augmentation is commonly used in image classification or pattern detection applications when dealing with a limited dataset, such as in medical imaging. Commonly used augmentation techniques include basic image manipulations (flip, rotate, scale, crop, and translate).

## Validation

Although validation is another heavily technical concept, a broad understanding will help you create and implement your data strategy.

In short, your AI application’s machine learning algorithms require data validation steps to ensure data quality, usefulness, and accuracy. Validation increases confidence that the data being consumed is clean and ready for use by ensuring it meets minimum expectations.

Data validation can be broken down into three substages: (1) structural conformance, (2) expectation conformance, and (3) advanced conformance, where each substage is dependent upon the previous substage (Table 4-3). Teams should evaluate errors discovered during conformance checks at any stage in case there are downstream impacts that must be addressed before data is made available to support AI needs.

## Table 4-3. Data validation stages

|  | Structural conformance | Expectation conformance | Advanced conformance |
|---|---|---|---|
| Description | A primary check to ensure data is well formed before attempting to validate the meaning of the individual data elements/across data elements. Validating against the value of the data is not a part of data structure validations. | Ensure that values within the data fall within anticipated parameters. | Aggregated functions of the data can be considered covering two or more fields and are validated in relation to each other. |
| Examples | Fields in the dataset match a set of expected columns. Number of records meets the expected range. | **Type:** Each field or attribute may be validated against the expected data type. **Uniqueness:** Each field may have uniqueness. **Requiredness:** Whether a field or attribute must have a value. **Sets and ranges:** Each field may be validated against the sets and ranges of values. **Format matching:** Each field may be validated against an expected pattern. | **Cross validation:** Each field or attribute may be validated in relation to another field or attribute, typically in the same instance (e.g., row, object). **Aggregate functions:** When there is an expectation of a function of values for a field, rather than an expectation of values, it may be validated against that function. |

## AI Data Governance

Data governance provides oversight for organizations, processes, and procedures to facilitate successful data strategy implementation. AI requires
