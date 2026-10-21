![](bbox=[0.125, 0.18, 0.71, 0.35])

The first time that a service is imported to Jenkins X (1) a pipeline instance is created to build all the artifacts required for the service to run (2). Once the artifacts are built the pipeline sends a notification to all the interested components (3). By default that notification is transformed in a Pull Request to the Staging Environment Git Repository (4). This Pull Request triggers the environment pipeline of the Staging Environment, and if everything works as expected, the service is promoted (deployed) to the Staging Environment (5).

Figure 3.X Agenda Service imported to Jenkins X

After importing your project, you have a Service Pipeline configured and ready to go. Your project now contains all the necessary configurations and definitions to be built, packaged and run into a Kubernetes Cluster. Jenkins X took care of creating the Docker Image, the Helm Chart and the Service Pipeline for you. Because Jenkins X is creating a Tekton pipeline under the hood you can change and modify this pipeline if the one provided by default doesn't meet all your needs.

# FROM SOURCE TO SERVICE RUNNING

Once the Service Pipeline is created you can start the pipeline by running `jx pipeline start` and then select your service from the list. You will notice that when the pipeline runs it will create a new tag and a new release for your artifacts, which will be automatically promoted to the Staging Environment.