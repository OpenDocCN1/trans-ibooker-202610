+   - A semantic model
+   - A copy job
+   - A lakehouse

We’ll use this scenario throughout this section to help you understand how to create, configure, and use deployment pipelines in Microsoft Fabric.

# Creating a Deployment Pipeline

You create a deployment pipeline in Microsoft Fabric “outside” a workspace. A deployment pipeline cannot exist inside a workspace because it copies items and content from a workspace. If it existed inside a workspace, it would copy itself in each deployment.

You can find the “Deployment pipelines” button in the main menu in Microsoft Fabric (see Figure 8-31).