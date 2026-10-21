# 3

# Service and Environment Pipelines

# This chapter covers

+   - The components needed to deliver Cloud Native applications to Kubernetes
+   - The advantages of creating and standardizing Service Pipelines for your services
+   - The benefits of creating Environment Pipelines to manage your environments such as Dev, Staging, Prod
+   - How projects like Tekton and Jenkins X help you to deliver software more efficiently

In the previous chapter, you installed and interacted with a simple distributed Conference Platform composed of 4 services. This chapter covers what it takes to deliver each of these components in a continuous delivery fashion by using Pipelines as delivery mechanisms. This chapter describes and shows in practice how each of these services can be built, packaged, released and published so they can run in your organization’s environments.

This chapter introduces two main concepts: Service Pipelines and Environment Pipelines. Where the Service Pipeline takes care of all the steps needed to build your software from source code until the artefacts are ready to run. Environment Pipelines, in the other hand, covers the aspects of dealing with the installation and upgrade of new versions of each of these services into a live environment such as staging, testing and production.

This chapter is divided into 3 main sections:

+   - What does it take to deliver a Cloud-Native application?
+   - Pipelines
+     - What is a Service Pipeline?
+     - What is an Environment Pipeline?
+   - Cloud-Native Implementations
+     - Tekton as the Cloud-Native Pipeline Engine
+     - Jenkins X (http://jenkins-x.io) as a one stop shop for CI/CD in Kubernetes