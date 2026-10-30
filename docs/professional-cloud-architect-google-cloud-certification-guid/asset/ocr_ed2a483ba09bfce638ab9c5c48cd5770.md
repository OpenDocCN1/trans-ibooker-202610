+   - **Dialogflow:** This is a service that allows you to build conversation applications that can interact with human beings. The interface can interact with many compatible platforms, such as Slack or Google Assistant. It can also integrate with **Firebase** functions to integrate with third-party platforms using common APIs.
+   - **AI Platform:** This was a fully managed service to allow the E2E development of a machine learning model. Before it went to general availability, Google released Vertex AI and deprecated AI Platform.
+   - **Vertex AI:** This is a unified machine learning platform to build, deploy, and scale AI models. It integrates AutoML and AI Platform together into a unified API, client library, and user interface. With Vertex AI, you can perform both AutoML training and custom training. For both of those options, you can save models, deploy models, and request predictions using Vertex AI.

ML services will be discussed further in *Chapter 14, Putting Machine Learning to Work.* In the next section, we will introduce *Google's identity and access management* services.

# Learning about identity services

**Identity and Access Management (IAM)** is one of the most important aspects of any cloud. It allows you to control who has access to the cloud but can also provide identity services to your applications. In short, this is achieved by a combination of roles and permissions. The roles are assigned to either users or groups. Let's have a look at the options we have in GCP:

+   - **IAM:** IAM allows the GCP admin to control authorization to GCP services. Administrators can create roles with granular permissions. Roles can then be assigned to users, or preferably, a group of users.
+   - **Cloud Identity:** Cloud Identity is an **Identity-as-a-Service (IDaaS)** offering. It sits outside of GCP but can be easily integrated with GCP. It allows you to create organizations, groups, and users, and manage them centrally. If you already have an existing user catalog, you can synchronize it with Cloud Identity.

IAM is very important to understand because it underpins how your GCP architecture will be created. We will discuss this more in *Chapter 15, Security and Compliance.*