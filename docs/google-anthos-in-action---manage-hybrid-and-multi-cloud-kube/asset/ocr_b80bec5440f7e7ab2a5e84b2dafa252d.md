application code.

# 14.3.1 Cloud Source Repositories

Cloud Source Repositories[^21] are fully featured, private Git repositories hosted on Google Cloud. The service helps developers to privately host, track, and manage changes to large codebases on Google Cloud Platform. It’s designed to integrate easily with GCP services like Anthos, GKE, Cloud Run, AppEngine and Cloud Functions. You can configure an unlimited number of repositories, and also mirror Bitbucket and GitHub repositories. Changes in Cloud Source Repositories are monitored and can trigger event notifications to Cloud Pub/Sub or a Cloud Function. One of the differentiators of Code Source Repositories is that you can use regex expression to search[^22] for phrases in your repository. Cloud Source Repository audit logs are available in Cloud Operations so you always know who and when has accessed you repository.

**Figure 14.25 Cloud Source Repositories integrations**