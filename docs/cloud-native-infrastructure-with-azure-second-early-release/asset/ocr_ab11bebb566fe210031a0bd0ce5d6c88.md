```
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8
app:app
```

+   - Build and push your new serverless image using:
+   
+     ```
+     docker push
+     https://myharborinstallation.com/helloworld:v1
+     ```

+   - Now define your serverless service in a file called *hello-world.yaml*:
+   
+     ```
+     apiVersion: serving.knative.dev/v1
+     kind: Service
+     metadata:
+       name: helloworld
+       namespace: default
+     spec:
+       template:
+         metadata:
+           name: helloworld
+         spec:
+           containers:
+             - image: docker.io/test/helloworld:v1
+     ```

+   - Deploy the function using:
+   
+     ```
+     kubectl apply -f hello-world.yaml
+     ```

+   - At this time, your serverless function will be deployed. You will be able to find the URL for the function by running:
+   
+     ```
+     $ kubectl get ksvc hello-world --output=custom-columns=NAME:.metadata.name,URL:.status.url
+     ```