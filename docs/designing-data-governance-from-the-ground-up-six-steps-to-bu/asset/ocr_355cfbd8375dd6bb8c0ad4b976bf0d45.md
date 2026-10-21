requirements gathering as early in the process as possible, well before beginning a data migration or deploying data to production. That way, the data team in charge of your production environment knows which security standards to uphold.

Security requirements broadly fall within three categories:¹¹

+   - Confidentiality: controlling who gets to read information
+   - Integrity: ensuring that information and programs are changed only in a specified, authorized manner
+   - Availability: confirming that authorized users have continued access to information and resources

It helps to track these security requirements using a security requirements traceability matrix (SRTM). This matrix shows security requirements, their implementation schedule, and essential resources you’ll need to succeed. The SRTM is a useful tool for your security steward to own and maintain as your project’s work progresses. It should show test cases, test runs, test results, bugs, and defect resolution status.¹²

Depending on your project, you’ll add case-specific best practices as well. If you’re managing a database, you’ll add tasks like segregating data from the same server as your website, implementing HTTPS for website encryption, generating backups, implementing firewalls, and deploying full-stack observability systems.

Security includes managing metadata throughout production. Your data team will need to track metrics that include data sharing, usage, and permissions per dataset. Alongside the SRTM, your security data steward can use the data catalog your council created to track which metadata you can use for which purposes. Metadata management is a crucial yet often overlooked part of data governance. It’s treated as additional information that exists outside the data at hand.

The truth is that when managed as a strategic asset, metadata boosts security in key ways. Cataloging metadata keeps it in one place where it’s less likely to go missing or exist on several servers. It also ties usage restrictions and data licensing to specific data, which creates data ownership records. From a security standpoint, metadata cataloging creates records for data usage, access, and updates.

11. https://nap.nationalacademies.org/read/1581/chapter/4
12. https://oasas.ny.gov/system/files/documents/2022/04/requirements_traceability_matrix.pdf