```
flat = flatten_json_v2(stadium_address)
json_normalize(flat['df'])
```

|   | Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street Name | Location_Address_Number |
|---|------|---------------|------------------|----------------|------------------------------|-------------------------|
| 0 | Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

```
json_normalize(test['Dining'])
```

|   | Dining | Dining_pk | Name |
|---|--------|-----------|------|
| 0 | The Sweet shop | 0 | Angel Stadium |
| 1 | Big A Burger | 1 | Angel Stadium |

Figure 9.37: Verifying the workings of the updated flatten function

We have gone through this exercise to showcase that with pandas and Python, we can perform powerful data transformation to suit our needs. However, this logic can be further developed and fine-tuned.

# CHALLENGE

Can you also update the script to process nested lists? One example could be that a stadium has a list of food outlets and each food outlet has a list of menu items.

# HINT

You can try to store the primary key separately for each table in the hierarchy and then add them to the child table along with the iterator column specific to that table.

In the next section, will learn more about using pandas’ operations for performing data cleaning activities.

# Data cleaning

Data cleaning is an important step in the process of data wrangling. A good amount of time is spent on identifying the right data source and cleaning the data. Pandas provides a lot of functionalities for cleaning your data.

The exact activities that are required during this phase are different for each type of dataset. Certain data sources will have data that requires only minimal cleaning and certain other data sources might require a lot of cleaning activities before the dataset can be used in your project. You could also use the output of data exploration activities to understand the level of cleaning activities to be performed on the data.

# Data cleansing with Pandas