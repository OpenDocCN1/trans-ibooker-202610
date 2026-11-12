# # Level 1 Wrap-Up: How Does It Work?

Figure 5-1 illustrates the four main components of the MVP along with a basic registration and usage workflow.

![](bbox=[0.145, 0.16, 0.855, 0.53])

Figure 5-1. The minimal viable self-service platform for powering an event-driven data mesh

The data product owner creates a source connector (1) and grants it read permissions to a specific table in their team’s database. The connector starts up, reads the table schema, and automatically creates a schema (2) to register with the schema registry. Next, the connector incrementally reads the source table rows and converts it automatically into events that match the registered schema. The events are then written into the event stream associated with that table (3).

The data product owner can then choose to register the event stream as a data product (4). Using their write-access permissions, they add the governance-required metadata to the spreadsheet, tag themselves as the owner of that product, and complete any other mandatory fields. They also link the metadata in the spreadsheet to the URL of the schema stored in the event broker (5). The schema registry is used not only for serializing and deserializing events but also for data discovery purposes for human users by answering the question “What is the format of data in this event stream?”