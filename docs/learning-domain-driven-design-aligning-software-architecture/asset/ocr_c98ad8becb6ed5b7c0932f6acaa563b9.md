wide boundaries into microservices. As we defined in Chapter 14, throughout the evolution of the bounded contexts, we stayed in the range of the safe boundaries. We were able to avoid going past the safe boundaries by doing the refactoring only after gaining enough knowledge of the business domain.

# Conclusion

In the stories of Marketnovus’s bounded contexts I showed how our understanding of domain-driven design evolved through time (refer to Figure A-6 for a refresher):

+   - We always started by building a ubiquitous language with the domain experts to learn as much as possible about the business domain.
+   - In the case of conflicting models, we decomposed the solution into bounded contexts, following the linguistic boundaries of the ubiquitous language.
+   - We identified the subdomains’ boundaries and their types in each bounded context.
+   - For each subdomain we chose an implementation strategy by using tactical design heuristics.
+   - We verified the initial subdomain types with those resulting from the tactical design. In cases of mismatching types, we discussed them with the business. Sometimes this dialogue led to changes in the requirements, because we were able to provide a new perspective on the project to the product owners.
+   - As more domain knowledge was acquired, and if it was needed, we decomposed the bounded contexts further into contexts with narrower boundaries.