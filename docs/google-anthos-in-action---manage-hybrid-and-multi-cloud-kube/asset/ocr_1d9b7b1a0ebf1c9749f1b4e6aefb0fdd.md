performance testing.

# Summary of Kustomize

As we have seen Kustomize can be a very powerful tool in your CI/CD pipeline to generate and patch Kubernetes resources for each of your environments. If you feel like you want to learn more about how to use Kustomize check out the Kustomize examples:
https://kubectl.docs.kubernetes.io/guides/example/ and the API reference:
https://kubernetes-sigs.github.io/kustomize/api-reference/ and the API reference: https://kubectl.docs.kubernetes.io/references/

# 14.4 Continuous Deployment with Cloud Deploy

Cloud deploy is a fully managed Continuous Deployment service that allows you to deliver your applications to a defined series of *target* environments according to a promotion sequence.

The *lifecycle* of your applications are managed using *releases* and are controlled by *delivery pipelines.*

# 14.4.1 Cloud Deploy in the Anthos CI/CD

Google Cloud deploy integrates with Google Cloud Platform ecosystem of services, that we have already learned about, to complete the end to end CI/CD solution for GKE and Anthos clusters.

Figure 14.35 Cloud Deploy CI/CD ecosystem