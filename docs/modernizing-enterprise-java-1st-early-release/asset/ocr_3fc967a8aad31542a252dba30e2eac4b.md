+   - Are third-party vendor components supported in containers?
+   - Is the application under active development?
+   - Does the application have any legal requirements (e.g. PCI, HIPPA)?
+   - Does the application provide metrics?

We strongly recommend looking at Pathfinder to manage large-scale modernization projects across complete landscapes. It will help you categorize and prioritize applications in your scope today and continuously track your migration assessment for future changes.

# Tackle Controls

Controls are a collection of entities that add different values to the Application Inventory and the Pathfinder assessment. They comprise business services, stakeholders, stakeholder groups, job functions, tag types, and tags. In addition, you can capture company or project-specific attributes by implementing your own entities. This will filter your application inventory by for example all applications used by a certain “job function”. E.g. to identify all applications used by the human resources department.

# Tackle DiVA

Finally, DiVa is a data-centric application analysis tool. As a successor to the project WindUp, it is the most exciting project to look at if you want to assess individual applications. It focuses on the traditional monolithic application and currently supports Servlets and Spring Boot applications. You can import a set of application source files (Java/XML) and DiVA then provides the following:

+   - Service entry (exported API) inventory
+   - Database inventory
+   - Transaction inventory
+   - Code-to-Database dependencies (call graphs)