```
kubectl patch sa default -n application-system -p ""imagePullSecr
```

8. Finally, for each namespace that you want to deploy an application to, you must create a new Secret and annotate the namespace with that Secret with the following command:

```
JSON_KEY_FILENAME=path_to/service_account_key.json
IMAGEPULLSECRET_NAME=gcr-json-key
kubectl create secret docker-registry $IMAGEPULLSECRET_NAME \
  --namespace=$NAMESPACE_NAME \
  --docker-server=gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat ~/$JSON_KEY_FILENAME)"
```

```
kubectl annotate namespace $NAMESPACE_NAME marketplace.cloud.goog
```

9. Once $NAMESPACE_NAME is defined as explained in the previous step, this namespace can be used to install on your on-prem cluster.

In other words, deploying on Anthos whether on GCP or on prem is just a matter of defining the appropriate namespace on your target Kubernetes clusters and annotating the namespace with the secret keys needed for pulling images for your solutions.

In the next section, we describe how to install a number of predefined solutions belonging to different categories including modernization tools, databases, monitoring and logging, CI/CD, productivity and machine learning.

# 17.2 Real world scenarios

In this section, we briefly describe how to deploy Marketplace solutions available for Anthos. Our examples are intentionally heterogeneous and belong to different categories. Please note we have already deployed Migrate for Anthos in Chapter 18, Migrate for Anthos, from Cloud Marketplace. Therefore we will skip it here but it is a very solid use case. Let’s see a few additional examples.

# 17.2.1 Example 1: ElasticSearch