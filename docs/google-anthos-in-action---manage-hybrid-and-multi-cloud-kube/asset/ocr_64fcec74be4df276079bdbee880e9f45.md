# # 14 Anthos, integrations with CI/CD

**by Konrad Clapa and Jaroslaw Gajewski**

# ## This chapter covers

+   - Understanding CI/CD concepts
+   - Automating a Continuous Development workflow
+   - Introducing Continuous Integration for your Anthos application
+   - Using Cloud Deploy to manage Continuous Deployment
+   - Understanding modern CI/CD platforms

In this chapter we will guide on how to develop and deploy Anthos applications. To simplify this we will use a simple “Hello world” application. We will use both examples for **Python** and **Go** language. We will go through the entire software workflow. This will start with Continuous Development where we will learn how we can start developing an Anthos application and preview them even before we commit the code to the Git repository. We then look at Continuous Integration and finally we will discuss Continuous development/delivery.

We need to realize that you might have three different personas that interact with the CI/CD pipelines. Those personas are:

+   - **Developers:** develop the application code
+   - **Operators:** configure the application deployments using Kubernetes manifests
+   - **Security:** configures the Policies to me the Kubernetes deployments secure

In this chapter we will concentrate on the 2 first personas. If you want to learn more about the Security persona that deals with the Policies please refer to the *Configuration management chapter*.

**Figure 14.1 CI/CD workflow**