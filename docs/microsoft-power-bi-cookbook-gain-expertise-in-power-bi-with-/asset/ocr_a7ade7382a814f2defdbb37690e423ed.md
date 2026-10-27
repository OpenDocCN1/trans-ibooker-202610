+   3. Click on the **Top segments** tab to view the different segments identified
+   4. Click on a segment of interest to view additional information and visualizations

![](bbox=[0.198, 0.131, 0.868, 0.32])

Figure 9.15: Segment detail in the Key influencers visualization

# How it works

The **Key influencers** visual uses machine learning algorithms to analyze the data added to the visual and extract insights based upon these algorithms. Different visuals and insights are created depending on whether the metric has categorical or continuous key influencers. Using an unsummarized numeric field in the **Analyze** field well allows **Analysis** in the **Format** pane to be changed to either **Categorical** or **Continuous**. Using a measure or an aggregated column forces a **Continuous** analysis. Measures can be used as either the metric to be analyzed or as a key influencer (**Explain by**).

Counts can be activated in the **Analysis** section of the **Format** pane. Counts are useful for determining whether key influencers have a large impact but only represent a small portion of the total data. Enabling counts creates a ring around each of the key influencers circles that represents the percentage of data rows in which the key influencer appears. A ring around the entire circle means that 100% of the data contains the key influencer.

The **Top segments** tab analyzes the data in order to find clusters of similar data points. Again, machine learning clustering algorithms are used in order to find and extract these segments from the underlying data used in the visualization.

The **Key influencers** visualization has certain limitations, primarily related to the data sources supported and where the visual can be displayed. For example, the Key influencers visual does not support DirectQuery or live connections to Azure Analysis Services or SQL Server Analysis Services. In addition, the Key influencers visualization is not supported when using **Publish to web** or embedding in SharePoint Online.