
```
sporting_event_temp1=pd.merge(ticket_purchase_hist,sporting_event_ticket,left_on="sporting_event_ticket_id",right_on="id", suffixes=('_tph', '_ht'))
sporting_event_temp1.head(5)
```

```
sporting_event_temp2=pd.merge(sporting_event_temp1,seat,left_on=["sport_location_id","seat_level","seat_section","seat_row"], suffixes=('_tph', 'seat'))
sporting_event_temp2.head(5)
```

|  | sporting_event_ticket_id | purchased_by_id | transaction_date_time | transferred_from_id | purchase_price | id | sporting_event_id | sport_location_ |
|---|---|---|---|---|---|---|---|---|
|0|1|3299597|2022-12-15 04:27:27|None|47.02|1|1| |
|1|2|3299597|2022-12-15 04:27:27|None|47.02|2|1| |
|2|22|3299597|2022-12-15 04:27:27|None|47.02|22|1| |
|3|65536|410959|2022-12-07 03:23:29|None|43.13|65536|2| |
|4|65536|5909803|2022-12-15 04:27:30|None|43.13|65536|2| |

Figure 9.71: Joining the sporting event table with ticket purchase history to get seat details

With this new data, we will visualize the seat category in our earlier scatterplot as well. We will add seat_type as a hue parameter, which will color the data points based on seat type in our graph, as shown here:

```
import seaborn as sb
plt.xticks(rotation=90)
scatter_plot1=sb.scatterplot(x='transaction_date_time', y='purchase_price',hue='seat_type', data=sporting_event_temp2, ma
```

![](img/ec1933a597ce63afa3bcf7496364f412_344_0.png)

Figure 9.72: Ticket purchase history visualization along with seat type

This gives us the idea that luxury seats are costly and only sold infrequently, and premium seats and standard seats are sold a lot.

Let us explore this further by understanding the average ticket price for each seat type category. We will draw a bar plot for this use case. But before that, we need to understand the data type of the purchase_price column from the pandas DataFrame:

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 5431 entries, 0 to 5430
Data columns (total 15 columns):
#   Column                     Non-Null Count  Dtype
---  ------                     --------------  -----
0   sporting_event_ticket_id   5431 non-null   Int64
1   purchased_by_id            5431 non-null   Int64
2   transaction_date_time      5431 non-null   datetime64[ns]
3   transferred_from_id        0 non-null      object
4   purchase_price             5431 non-null   float64
5   id                         5431 non-null   Int64
6   sporting_event_id          5431 non-null   Int64
7   sport_location_id          5431 non-null   Int64
8   seat_level                 5431 non-null   Int64
9   seat_section               5431 non-null   string
10  seat_row                   5431 non-null   string
11  seat                       5431 non-null   string
12  ticketholder_id            5431 non-null   Int64
13  ticket_price               5431 non-null   object
14  seat_type                  5431 non-null   string
dtypes: Int64(7), datetime64[ns](1), float64(1), object(2), string(4)
memory usage: 716.0+ KB
```

Figure 9.73: pandas DataFrame info function to understand the data type of columns

We could see the purchase_price column is of the object type, which will throw an error when aggregated on a bar plot. Let us convert it into a float object type and then create the bar plot. We will also add the purchase_price value on top of the bars to understand the price range of tickets better, as shown here:

```
sporting_event_temp2["purchase_price"]=sporting_event_temp2.purchase_price.astype(float)
ax=sb.barplot(x='seat_type',y='purchase_price', data=sporting_event_temp2)
for i in ax.containers:
    ax.bar_label(i,)
```

![](img/ec1933a597ce63afa3bcf7496364f412_345_0.png)

Figure 9.74: Bar plot showing the average purchase price for each ticket type

This graph is fine, but it would still be better if we could sort the graph in the order of purchase_price for us to visualize it better. So, we will sort the DataFrame by purchase price first and reset the pandas DataFrame index. We will also draw the graph in a more presentable manner with the alpha parameter to adjust the transparency of the bar plot.

```
sporting_event_temp2["purchase_price"]=sporting_event_temp2.purchase_price.astype(float)
sporting_event_temp2 = sporting_event_temp2.sort_values(["purchase_price"]).reset_index(drop=True)
ax=sb.barplot(x='seat_type',y='purchase_price', data=sporting_event_temp2, alpha=0.5)
for i in ax.containers:
    ax.bar_label(i,)
```

![](img/ec1933a597ce63afa3bcf7496364f412_346_0.png)

Figure 9.75: Sorted bar plot to understand the seat type and average price for that category of seats

Now we are able to visualize the impact of different seat types on the pricing band for tickets in a clearer manner. We can explore further how to maximize ticket sales with discounts on certain seat types based on the number of tickets that are sold for each seat type.

There are many more visualizations that are supported by these libraries and you can use the links provided earlier on these libraries to learn more about them. For most use cases, the preceding libraries should suffice for visualizing user data. Also, we recommend using Amazon QuickSight to visualize more complex use cases.

## Summary

In this chapter, we learned how to perform pandas operations on datasets for data-wrangling purposes. We explored various stages of the data-wrangling life cycle, through discovery, structuring, cleansing, enriching, data quality validation, and visualization, and the usage of pandas operations to perform those activities seamlessly. Users can use AWS SDK for pandas, aka awswrangler integration, with pandas DataFrames to perform data-wrangling activities on AWS cloud services.

In the next chapter, we are going to learn about SageMaker Data Wrangler, which helps in performing data-wrangling activities as a part of ML pipelines.

## Data Processing for Machine Learning with SageMaker Data Wrangler

In *Chapter 4*, we introduced you to SageMaker Data Wrangler, a purpose-built tool to process data for machine learning. We discussed why data processing for machine learning is such a critical component of the overall machine learning pipeline and some risks of working with unclean or raw data. We also covered the core capabilities of SageMaker Data Wrangler and how it is helpful to solve some key challenges involved in data processing for machine learning.

In this chapter, we will take things further by taking a practical step-by-step data flow to preprocess an example dataset for machine learning. We will start by taking an example dataset that comes preloaded with SageMaker Data Wrangler and then do some basic exploratory data analysis using Data Wrangler built-in analysis. We will also add a couple of custom checks for imbalance and bias in the dataset. Feature engineering is a key step in the machine learning cycle. It includes selecting and transforming raw data features into new features that better represent the underlying problem for the machine learning model. We will create a Data Wrangler flow and define data transformations on our sample dataset to do feature engineering. SageMaker Data Wrangler is a unified data processing tool enabling you to do a complete machine learning cycle, including training a machine learning model and deploying it for inference directly from within the tool. We will export our processed data and train and deploy a model from Data Wrangler.

This chapter covers the following sections:
- Logging in to SageMaker Studio
- Importing data
- Exploratory data analysis
- Adding transformations
- Exporting data
- Training a machine learning model

## Technical requirements

If you wish to follow along, which I highly recommend, you will need an **Amazon Web Services** (**AWS**) account. If you do not have an existing account, you can create an AWS account under the Free Tier. The AWS Free Tier provides customers with the ability to explore and try out AWS services free of charge up to specified limits for each service. If your application use exceeds the Free Tier limits, you simply pay standard, pay-as-you-go service rates. In this chapter, we will get started by looking at how to access and get familiar with the SageMaker Data Wrangler user interface. As you follow along, you will use AWS Compute and also end up creating resources in your AWS account. This especially applies to the *Training a machine learning model* section of the chapter, which is both compute-intensive and creates an endpoint that you will have to delete. Please remember to clean up by deleting any unused resources. We will remind you again at the end of the chapter; for now, let’s get started.

## Step 1 – logging in to SageMaker Studio

In this section, we will cover the steps to log in and navigate inside the AWS console and SageMaker. If you are already familiar with using SageMaker, you can skip this section and move on directly to the next one.

After you have created your account and set up a SageMaker Studio domain and created a user, as covered in *Chapter 4*, you can log in to the AWS console and choose **SageMaker**. You can either navigate to SageMaker in the **All Services** section under **Machine Learning** or start typing `SageMaker` in the search box at the top of the AWS console.

![](img/ec1933a597ce63afa3bcf7496364f412_349_0.png)

Figure 10.1: AWS console – SageMaker
