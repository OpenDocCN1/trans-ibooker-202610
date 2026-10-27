+   - Push the new image to the registry

For example, to add a CentOS 8 image to a registry, follow the steps below:

+   1. Download the CentOS 8 image from Docker hub

```
docker pull centos:8
```

+   2. Next, tag the newly pulled image with the Google registry information. When you tag an image that will be stored in a GCP registry, you must follow a specific naming convention. The image tag will use the convention <registry location>-.pkg.dev/<project_ID>/<registry-name>/<image_name>. In the example, the region is us-east4, using project test-236415 and a registry named docker-registry.

```
docker tag centos:8 us-east4-docker.pkg.dev/test1-236415/docker-r
```

+   3. Finally, push the new image to the registry

```
docker push us-east4-docker.pkg.dev/test1-236415/docker-registry
```

In the next section, we will explain how to look at your images and any vulnerabilities that have been found in them.

# 16.5.3 Reviewing Image Vulnerabilities

Since our project has the required API’s enabled, each image will be scanned when it is pushed to the registry. To review the vulnerabilities, open the GCP console and click on Artifact Registry and then repositories under the tools section.

Figure 16.4 Navigating to your Registries