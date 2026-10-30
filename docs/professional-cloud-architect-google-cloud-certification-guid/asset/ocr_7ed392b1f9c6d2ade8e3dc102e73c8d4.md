4. Click **Customize Pipeline** when prompted. This will take us to our pipeline studio. This is a graphical interface for developing our pipeline. It allows us to drag and drop various plugins onto our canvas. If we click **Deploy** from the top right, it will submit the pipeline to Data Fusion. In this example pipeline, we are reading a JSON file containing New York Times bestseller data from Cloud Storage. It will then run transformations on the file to parse and clean the data. Finally, it will load the top-rated books that have been added in the last week that cost less than $25 into BigQuery:

![](bbox=[0.11, 0.26, 0.85, 0.63])

Figure 13.39 – Deploying the pipeline

5. We can then **Run** our pipeline and see the results at the end of the process:

![](bbox=[0.11, 0.69, 0.85, 0.87])

Figure 13.40 – Pipeline results