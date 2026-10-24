# Utility trees

For the sake of brevity, we will skip the typical quality attributes, such as usability, deployability, and maintainability, that would normally be part of the exercise, whether explicitly expressed or not by the business.

Let's focus on what was explicitly expressed, and craft the following utility tree:

![](bbox=[0.22, 0.24, 0.73, 0.66])

Figure 3.1 – Utility tree

Figure 3.1 represents a simplified view of what we could capture out of our business scenario. Each leaf of the tree represents an architecture significant requirement (ASR). We decided to focus on availability, scalability, performance, and interoperability. Regarding availability, we focused on both the upload channel and the data services. As you can see, the upload channel is marked with a (H/H) annotation, where H stands for High; possible values are Low, Medium, and High. This quantification represents the impact on the business (left side) and on the architecture (right side).

This concretely means that this requirement is highly important for the business and is considered high-risk for the architecture. To put this differently, this will not be our low-hanging fruit and we will have to pay close attention because it will not be easy to achieve.