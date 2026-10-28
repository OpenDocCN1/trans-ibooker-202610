+   - Are there test cases for scalability of data, users, and hardware?
+   - Is there penetration testing?
+   - Is there usability testing?

# 12.6 Traceability

Traceability is a means of verifying that no requirement, use case, feature, or test case has been omitted from the development plan execution. It is a critical step in assuring that the product is delivered as expected. Traceability verifies the linkage from requirements to design to implantation to quality assurance.

Software work products form a hierarchical tree. At the top are requirements. The next level down is architecture and design. Below that is code. At the bottom are test cases. The traceability task makes sure that for each requirement, there exists a design, code, and test cases. This ensures that no requirement go undesigned, unimplemented, or untested.

Traceability is usually both its own process step and integrated into the review process for each work product. Generally, a traceability matrix is created that lists each requirement with its derivative designs, code, and test cases. During each review the matrix is updated to list the associated work products. When all the other reviews are complete, a final review is held of the traceability matrix itself to verify that there are no missing linkages.

Traceability reviews may seem like a tedious bureaucratic process step; I felt that way when I first was introduced to them. However, I learned otherwise when I was surprised to find during a traceability review that one of my projects had missed several requirements.

# 12.7 Defect Tracking

It almost goes without saying that the quality of the software cannot be known unless the defects found in the software are tracked. If a record of each defect is not kept, then there is no way to know how many defects were found in the software nor if all the important defects were fixed. Defect tracking is critical to ensuring that the software quality meets its goals.

Lack of defect tracking, then, leads to unknown software quality. Moreover, none of the statistics mentioned in this book can be created without some sort of defect tracking. The number of escapes, the escape rate, and the defect density of the code are all unknowable unless each defect is recorded and monitored to resolution.

A defect tracking system (DTS), also known as a bug tracking system, is an application that records and monitors defect (bug) status. There are many open-source and commercial DTS available to facilitate defect tracking. All good DTS should have the following functionality:

+   - Defects should have status such that the user can tell if the defect is open, fixed, or verified. Other defect states could be supported to best support the software development and verification processes.