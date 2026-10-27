![](bbox=[0.2, 0.09, 0.83, 0.36])

To build your image from the configuration file you need to specify the build steps in you the cloudbuild.yaml file using the docker cloud builder:

```
steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'us-central1-docker.pkg.dev/$PROJECT_ID/'
images:
- 'us-central1-docker.pkg.dev/$PROJECT_ID/${_REPOSITORY}/${_IMAGE
```

Next, run the below command to submit the build:

```
gcloud builds submit --config [CONFIG_FILE_PATH] [SOURCE_DIRECTOR
```

This builds the image and stores it in Google Artifact Registry as indicated in the configuration file. If you don't specify the [CONFIG_FILE_PATH] and [SOURCE_DIRECTORY] parameters, the current directory will be used.

# Using a Dockerfile to build container image

You can create container images without using the configuration file as your Dockerfile contains all information needed to build a Docker image using Cloud Build.

Figure 14.28 Building image from Dockerfile