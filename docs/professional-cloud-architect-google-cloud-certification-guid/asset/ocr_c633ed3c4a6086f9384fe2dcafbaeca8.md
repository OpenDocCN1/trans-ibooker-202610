# Exam Tip

It might be worth remembering that we can do the following:

**Enable Cloud TPU**: We will discuss this in *Chapter 14, Putting Machine Learning to Work*.

**Enable Istio (beta)**: This is a service mesh product that allows us to control and observe network traffic.

8. Now that we have populated all the settings, we can create the cluster by clicking on the **Create** button. Alternatively, we can generate a REST API call or command line by clicking on the respective hyperlinks:

![](bbox=[0.28, 0.3, 0.68, 0.33])

Figure 6.33 – REST or command line

9. Before clicking on **Create**, let’s have a look at the command line by clicking on the **command line** link. As usual, you can just copy this into Cloud Shell and execute it. In the following code, you can see that we used some features that are available in beta mode, such as the `gcloud beta container` command:

```
gcloud beta container --project "prj-bul-d-sample-base-6159" clusters create "standard-cluster-1" --zone "us-central1-c" --no-enable-basic-auth --cluster-version "1.19.9-gke.1900" --release-channel "regular" --machine-type "e2-medium" --image-type "COS_CONTAINERD" --disk-type "pd-standard" --disk-size "100" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --num-nodes "3" --enable-stackdriver-kubernetes --no-enable-ip-alias --network "projects/prj-d-shared-base-de39/global/networks/vpc-d-shared-base" --subnetwork "projects/prj-d-shared-base-de39/regions/us-central1/subnetworks/sb-d-shared-base-us-central1" --no-enable-intra-node-visibility --no-enable-master-authorized-networks --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --enable-shielded-nodes --node-locations "us-central1-c"
```