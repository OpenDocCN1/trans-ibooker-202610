# 18.4.1 Using the fit assessment tool

In this section we present a self-service fit assessment tool used to determine the workload's suitability for migration to a container. The tool consists of a utility called *mfit* - a standalone Linux CLI tool used to drive the assessment process along with dedicated Linux and Windows data collection scripts that are invoked either automatically by *mfit* or manually - depending on the scenario.

The fit assessment tool generates a report presenting pre-VM assessment results including both a score for the VM’s suitability for migration to a container and recommendations on resolving various obstacles.

The table below provides a summary of possible fit scores:

# Table 18.1: Fit scores produced by the fit assessment tool

| Fit score | Description |
|---|---|
| Score 0 | Excellent fit |
| Score 1 | Good fit with some findings that might require attention. |
| Score 2 | Requires **minimal** effort before migrating. |
| Score 3 | Requires **moderate** effort before migrating. |
| Score 4 | Requires **major** effort before migrating |
| Score 5 | No fit. |