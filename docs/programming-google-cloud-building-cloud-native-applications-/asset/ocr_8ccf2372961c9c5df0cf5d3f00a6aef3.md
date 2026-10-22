+   1. If you cloned the repo before, you can skip this step: On your local workstation git clone the following repository:https://gitlab.com/building-cloud-native-applications-google-cloud/chapter-two.git
+   2. From the cloned repository, go to the chapter-two folder.
+   3. Open the uploadFile.js file in your favorite editor.
+   4. On line 1 replace with your bucket name
+   5. On line 2 replace with the file you want to upload
+   6. On line 7 replace the "key.json" with the service account key you downloaded.
+   7. On line 8 replace "ruicosta-blog" with your project id.
+   8. If you run npm install in a previous step within this folder you can skip this step: On the terminal within this folder run npm install
+   9. On the terminal run node uploadFile.js
+   10. On successful execution, it should look like Figure 2-10

![](bbox=[0.14, 0.38, 0.86, 0.84])

Figure 2-10. Code Screenshot for uploadFile.js

That was easy! We now programmatically uploaded a file to our bucket.

# Download the File