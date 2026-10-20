lets us force access or no access to specific columns in the model. This approach is feasible even though it’s not available in out-of-the-box tools.

43. Correct answer: C

You can analyze log data and streaming data with the eventhouse, a KQL database, and the KQL language. The other services can handle bulk loads with ease, but the only services in Microsoft Fabric that can handle both semi-structured and structured streaming data are the KQL database and the KQL language.

44. Correct answer: B

Spark queries are performed in a lakehouse in Microsoft Fabric for both semi-structured and structured data. This also lets you use both T-SQL and Spark queries to manipulate and query the data. We discussed this in Chapter 2.

You implement RLS in the database for T-SQL through the SQL analytics endpoint, as we discussed in Chapter 7.

45. Correct answers: A and C

Applying a sensitivity label from Fabric and configuring column-level security in the lakehouse meets the correct security requirements. You must implement column-level security in the SQL analytics endpoint as DENY SELECT or GRANT SELECT in a T-SQL statement with the needed columns as the requirement specifies.

46. Correct answer: B