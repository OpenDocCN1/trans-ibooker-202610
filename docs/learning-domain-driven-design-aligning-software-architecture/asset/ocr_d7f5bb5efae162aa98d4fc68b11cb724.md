This pattern organizes the system’s operations as simple, straightforward procedural scripts. The procedures ensure that each operation is transactional—either it succeeds or it fails. The transaction script pattern lends itself to supporting subdomains, with business logic resembling simple, ETL-like operations.

# Active record

When the business logic is simple but operates on complicated data structures, you can implement those data structures as active records. An active record object is a data structure that provides simple CRUD data access methods.

The two patterns discussed in this chapter are oriented toward cases of rather simple business logic. In the next chapter, we will turn to more complex business logic and discuss how to tackle the complexity using the domain model pattern.

# Exercises

+   1. Which of the discussed patterns should be used for implementing a core subdomain’s business logic?
+      a. Transaction script.
+      b. Active record.
+      c. Neither of these patterns can be used to implement a core subdomain.
+      d. Both can be used to implement a core subdomain.
+   2. Consider the following code: