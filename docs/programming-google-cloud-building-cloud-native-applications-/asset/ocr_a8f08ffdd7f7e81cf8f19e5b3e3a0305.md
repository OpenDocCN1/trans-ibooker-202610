![](bbox=[0.14, 0.09, 0.86, 0.65])

Figure 2-3. Command Line Usage of gsutil

Review Figure 2-2 and 2-3. What is different between the two? Not much except their content-type. gsutil displays a hierarchical file structure but the underlying Cloud Storage is a flare namespace. To the service, the object gs://sample-42892/folder/image.png is just an object that happens to have "/" character in its name. There is no "folder" directory; just a single object with the given name. If we try to run gsutil stat gs://sample-42892/folder, what would happen? It would fail, since we did not include "/" in the name, see Figure 2-4.