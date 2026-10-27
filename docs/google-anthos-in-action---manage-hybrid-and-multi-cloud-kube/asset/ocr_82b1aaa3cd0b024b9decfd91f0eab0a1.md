build section. Details about customer builders are at [https://skaffold.dev/docs/pipeline-stages/builders/](https://skaffold.dev/docs/pipeline-stages/builders/)

The following tools are currently supported:

+   - Docker
+   - Jib[^14]
+   - Bazel[^15]
+   - Buildpacks[^16]

You can also use custom scripts that are in line with the Skaffold defined standard.

# Using Skaffold in CI/CD pipelines

Skaffold can be used as a tool in your CI/CD pipelines. There is already an existing community-maintained builder [^17] that can be directly used with Cloud Build which is a native CI Google too (we will have a look at it in detail in the next section). Some of the most useful Skaffold commands in CI/CD workflows are:

+   - `skaffold build` - builds and tag your image(s)
+   - `skaffold deploy` - deploys the given image(s)
+   - `skaffold delete` - cleans up the deployed artifacts
+   - `skaffold render` - builds and tags images, and output templated Kubernetes manifests

# Skaffold summary

In this section we have learned how to install Skaffold and use it in a continuous development workflow. Follow the Skaffold quick start guide[^18] for more information on using Skaffold with development workflows.

# 14.2.3 Cloud Code: Development with local IDE

Till now we have learned how we can preview the Anthos application but