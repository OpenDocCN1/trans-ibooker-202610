+   - The call handlers are provided with a list of new customers to call each day. We can control which customers are on this list.
+   - Call handlers are organized into teams based on individual performance. We can control which teams call customers on which lists.
+   - There are different scripts available for call handlers to use. We can dictate which script to use.

Let’s start by focusing on the first point. The call center has data about millions of potential customers they can call. If we can ensure that we call only those customers who are likely to make a purchase, we’re going to increase our conversion rate. This is where DSML comes in.

# Training a Model

The basic challenge is this: given details about a new potential customer, we need to determine if they’re likely to make a purchase or not. Using DSML, we can use historical data to train a machine learning model to predict this for us.

In simple terms, we do this by preparing the data, utilizing an ML service, and training and testing a model, as discussed in the following sections.

# Preparing the data

First, we take our historical data and tidy it up. There’s almost always some issue with incomplete data, and before any ML processes are applied it’s a good idea to do some basic cleansing and preparation of the data, such as setting data types correctly. We might also create new calculations that may be useful to train our model, such as determining the age of a contact (in years) using their date of birth.

Most importantly, we need to make sure that the “sold” column is accurately and consistently stored, because this is the value we want to predict.