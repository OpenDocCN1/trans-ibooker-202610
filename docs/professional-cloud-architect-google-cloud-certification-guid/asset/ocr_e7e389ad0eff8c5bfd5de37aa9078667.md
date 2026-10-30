In such a case, we need to use AutoML or train our own model. As you have probably already guessed, the former is a much easier method. AutoML takes datasets from you, trains and deploys the model, and then serves it through the REST API. This sounds a little bit like magic, right? Take a look at the following diagram:

![](bbox=[0.22, 0.22, 0.8, 0.42])

Figure 14.20 – AutoML

Note that there are five services available that allow you to train your custom model:

+   - **AutoML Vision**: This classifies your images according to your own defined labels.
+   - **AutoML Translation**: This performs translation queries, returning results specific to your domain.
+   - **AutoML Natural Language**: This classifies English-language content into a custom set of categories.
+   - **AutoML Tables**: This turns structured data into predictive insights.
+   - **AutoML Video Intelligence**: This allows you to classify segments of video.

Let's have a look at an example. See how you would actually use AutoML, using the example of the Vision API, to recognize a table that your company produces. In short, what you would do is the following:

+   1. Take multiple photos of your table.
+   2. Upload it to Cloud Storage.
+   3. Create a CSV file with a label for your photos.
+   4. Provide the CSV file to AutoML to train the model.