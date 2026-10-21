+   - Flux CD
+   - Argo CD
+   - Spinnaker

While these tools are more generic and less opinionated, you will need to define with your team what conventions and best practices work for you. I strongly recommend you to check this and other tools that might be more focused to solve specific problems and then with the big picture of CI/CD for Kubernetes in mind, decide how to combine them to achieve Continuous Delivery.

# 3.4 Summary

+   - Service pipelines define how to go from source code to artifacts that can be deployed in multiple environments. In general, following Trunk-Based Development and One Service = One repository practices help your teams to standardize how to build and release software artifacts in a more efficient way.
+   - Environment Pipelines are in charge of deploying software artifact to live environments. Environment Pipelines avoid teams interacting directly with the cluster where the applications run, reducing errors and misconfigurations. Environment Pipelines
+   - Tekton is a Pipeline Engine designed for Kubernetes, you can use Tekton to design your custom Pipelines and leverage all the shared Tasks and Pipelines openly available in the Tekton Catalog. You can now install Tekton in your cluster and start creating Pipelines.
+   - If you want to learn about best practices, conventions and how a large Open Source project deals with the complexities of building and delivering cloud-native software you should look at Jenkins X, which provides a one-stop-shop for CI/CD on top of Kubernetes, by using Tekton as the Pipeline Engine.
+   - If you are interested in smaller projects that tackle more specific challenges around Environment pipelines you can look at Flux CD or Argo CD.