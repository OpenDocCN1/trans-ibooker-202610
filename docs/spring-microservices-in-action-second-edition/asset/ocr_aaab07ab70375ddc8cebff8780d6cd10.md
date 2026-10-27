# A quick note

For this book, we set up separate folders in the same GitHub repository for each chapter. All the source code for the chapter can be built and deployed as a single unit. However, for your projects, we highly recommend that you set up each micro-service in your environment with its own repository and independent build processes. That way, each service can be deployed independently of one another.

With the build process example, we deploy the configuration server as a single unit only because we want to push the projects to the Amazon cloud separately and to manage build scripts for each individual service. In this example, we only deploy the configuration server, but later, you can use the same steps to create other deployments.

Now that we’ve walked through the general steps involved in our build/deployment process with Jenkins, let’s look at how to create the Jenkins pipeline.

> **NOTE** To make the pipeline work, you need to install several Jenkins plugins such as GitHub Integration, GitHub, Maven Invoker, Maven Integration, Docker Pipeline, ECR, Kubernetes Continuous Deploy, and Kubernetes CLI. To learn more about these plugins, we recommend that you read the official Jenkins documentation for each (for example, for the Kubernetes CLI, see https://plugins.jenkins.io/kubernetes-cli/).

Once we’ve installed all the plugins, we need to create the Kubernetes and ECR credentials. First, let’s start with the Kubernetes credentials. To create these, we need to go to Manage Jenkins, Manage Credentials, Jenkins, Global Credentials, and then click the Add Credentials file. Select the Kubernetes Configuration (kubeconfig) option and fill in the forms with your Kubernetes cluster information.

To retrieve the information from the Kubernetes cluster, connect to the Kubernetes cluster as we previously explained when we manually deployed our services and then execute the following command:

```
```bash
kubectl config view
```
```

Copy the returned contents and paste that into the Content text box in Jenkins. Figure 12.20 shows this step.

Now, let’s create the ECR credentials. In order to do this, we first need to go to the IAM page in the AWS console, then go to Users and click the Add User option. In the Add User page, you need to specify a username, the access type, and the policies. Next, download the .CSV file with the user credentials and, finally, save the user. We will guide you step by step as we create the credentials. First, add the following data in the IAM page:

```
```text
User name: ecr-user
Access type: Programmatic access
```
```