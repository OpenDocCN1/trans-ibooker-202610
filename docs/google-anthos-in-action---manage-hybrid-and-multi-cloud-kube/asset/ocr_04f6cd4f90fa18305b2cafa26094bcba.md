```
gke:
  cluster: projects/[project_name]/locations/[location]/clust
```

The parameter **requireApproval: true** indicates whether promotion to this target requires manual approval. Can be true or false. This property is optional. The default is false.

+   10. Now you can either approve the or reject the rollout. To approve the rollout run the command:

```
gcloud deploy rollouts approve rollout-name --delivery-pipeline=p
```

or reject the approval by running:

```
gcloud deploy rollouts reject rollout-name --delivery-pipeline=pi
```

As you can see this gives you full control over your application rollouts.

# Summary of Cloud Deploy

In this section we have learned how Clou Deploy can be used for Continuous Delivery of your application. It comes with many important features like approvals, logging and integrations with 3rd party tools making it an enterprise grade solution. It seamlessly integrates with the end to end CI/CD pipeline for Anthos. To learn more about Cloud Deploy check: https://cloud.google.com/deploy

If you want to get hands on experience with Cloud Build we encourage you to take a look at the the example tutorials that cover even more features and integrations: https://github.com/GoogleCloudPlatform/cloud-deploy-tutorials/blob/main/tutorials/base/clouddeploy-config/skaffold.yaml.template.

# 14.5 Modern CI/CD Platform

Modern CI/CD platforms must allow sustainable software development and operation of the entire application delivery pipeline. There is no single way to achieve that goal and it is always dependent on application and organization specifics.