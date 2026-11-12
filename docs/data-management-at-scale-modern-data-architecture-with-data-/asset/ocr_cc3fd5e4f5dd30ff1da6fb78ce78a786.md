# # Domain Ownership Responsibilities

Federating ownership of your architecture among domains requires respecting some additional principles. In addition to your domains taking ownership of applications and data products from those applications, your domain teams have to take on additional responsibilities, such as:

+   - Taking ownership of data pipelines, such as ingesting, cleaning, and transforming data, to serve as many data customers’ needs as possible
+   - Improving data quality and respecting service level agreements (SLAs) and quality measures set by data consumers
+   - Encapsulating metadata or using reserved column names for fine-grained row/column-level filtering and dynamic data masking
+   - Adhering to metadata management standards, including:
+     - Application and source system schema registration
+     - Providing metadata for improved discoverability
+     - Observing versioning rules
+     - Linking data attributes and business terms
+     - Ensuring the integrity of metadata information to allow better integration between domains
+   - Adhering to data interoperability standards, including protocols, data formats, and data types
+   - Providing lineage, either manually or by linking source systems and integration services to scanners
+   - Completing data-sharing tasks, including identity and access management reviews and data contract creation

The principles from these sections can be respected strongly or loosely. For ad hoc, exploratory, experimental use cases, interfaces can be more volatile or less optimized for direct consumption. For stable production use cases, the principles should be closely followed. You can even mix these approaches by allowing combinations within a system: for example, one data product can be delivered in a way that strongly follows the design principles outlined here while another data product, probably a less important one, can be delivered more loosely.

Respecting these principles is essential because the new architecture facilitates data distribution and integration in different ways. If you want to become data-driven and foster data consumption, all of your domains must make their data and the way it’s provided a first-class concern. Domains must adhere to these principles and apply them to all of the communication patterns presented in the next chapters.