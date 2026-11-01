
such as `pivot`, `unpivot`, and so on.

## Flattening nested data with Pandas

Pandas provides capabilities for structuring your data by removing unwanted columns, renaming columns, and also changing the index as needed. This helps in changing the data format to a more standard format that is suitable for each use case. In addition, there are functions that we can utilize in Pandas for flattening nested data within a single column to better analyze the data.

In order to demonstrate our work, let us consider a sample stadium data where the address and dining options from the stadium are available. We do not have the data in our sample dataset and hence will use a sample value to show how JSON data flattening works. We will use the `json_normalize` ([https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html](https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html)) function to flatten the nested JSON data into columns for analysis. Now we can use this data to query the columns much more efficiently. You can query how many stadiums are available in the state of California by filtering by the **Location.State** column.

```python
stadium_address = {'Name':'Angel Stadium', 'Location':{'City':'Anaheim','Zipcode':'92806','State':'CA','Address':{'Street Name':'E Gene Autry Way','Number':'2000'}}, 'Dining':['The Sweet shop','Big A Burger']}
```

```python
from pandas import json_normalize
flat1=json_normalize(stadium_address)
flat1
```

| Name | Dining | Location.City | Location.Zipcode | Location.State | Location.Address.Street Name | Location.Address.Number |
|---|---|---|---|---|---|---|
| Angel Stadium | [The Sweet shop, Big A Burger] | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

Figure 9.30: Flattening hierarchical JSON data using the json_normalize function

However, we still have data in a list in the **Dining** column. If we wanted to have that column expanded, it would help us with our analysis further. For this, we need to create a function that will flatten both the dictionary and list data from a JSON string.

```
#Function referenced from https://towardsdatascience.com/flattening-json-objects-in-python-f5343c794b10
def flatten_json(y):
    out = {}
    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x
    flatten(y)
    return out
```

Figure 9.31: Function to flatten both the list and dictionary columns with JSON data

Now, you can see the output has both dictionary and list data being flattened from the JSON string. We have used the flatten_json function that we created in the preceding step to achieve the desired output:

```
flat = flatten_json(stadium_address)
json_normalize(flat)
```

| Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street Name | Location_Address_Number | Dining_0 | Dining_1 |
|---|---|---|---|---|---|---|---|
| 0 Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 | The Sweet shop | Big A Burger |

Figure 9.32: Flattening JSON data with list and dictionary columns

Let's take a moment and understand how the function works so that you can develop it further. We will add comments to the function and demonstrate the working of the flatten function. The function will be called recursively to flatten the JSON string:

```python
#For learning the working of the function.
def flatten(x, name='',out={}):
    print(x)
    a=""
    if type(x) is dict:
        for a in x:
            print("___________________")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_')
    elif type(x) is list:
        i = 0
        for a in x:
            print("*******************")
            print("List route " + name + str(i) + '_')
            flatten(a, name + str(i) + '_')
            i += 1
    else:
        print("====> Found a value for target column --> "+ x)
        out[name[:-1]] = x
```

Figure 9.33: Understanding the working of the flatten function by adding comments

We have added the print statement immediately in the function to understand the input string that is being passed. We have added print statements to each of the sections, which perform the following functions:

- Flattening dictionary objects
- Flattening list objects
- Identifying an atomic value to be stored in the target column

### How does the function work?

The function will iterate through the provided data and identify whether the current object is a dictionary or list:

- A dictionary object will have curly brackets and have a key-value pair structure, for example, : { "name":"Angel stadium", "state":"CA" }. Dictionaries can have dictionary or list objects within them.
- List objects will have similar data items that are repeated multiple times. They are stored within a square bracket structure. They can be used to store multi-valued attributes. In the preceding example, we stored data about multiple restaurants within the Angel stadium in a list – for example, : ["The sweet shop", "Big A burger"].

Once it identifies the object, it will call the function recursively if it's a dictionary or list within another object.

The following screenshot provides an illustration of how the function parses the data before providing the final output. Follow through each step and you can better understand the workings of this function:

```
flatten(stadium_address)
{'Name': 'Angel Stadium', 'Location': {'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}, 'Dining': ['The Sweet shop', 'Big A Burger']}
Dictionary route Name
Angel Stadium
====> Found a value for target column --> Angel Stadium
Dictionary route Location
{'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}
Dictionary route City
Anaheim
====> Found a value for target column --> Anaheim
Dictionary route Zipcode
92806
====> Found a value for target column --> 92806
Dictionary route State
CA
====> Found a value for target column --> CA
Dictionary route Address
{'Street Name': 'E Gene Autry Way', 'Number': '2000'}
Dictionary route Street Name
E Gene Autry Way
====> Found a value for target column --> E Gene Autry Way
Dictionary route Number
2000
====> Found a value for target column --> 2000
Dictionary route Dining
['The Sweet shop', 'Big A Burger']
********************
List route Dining_0_
The Sweet shop
====> Found a value for target column --> The Sweet shop
********************
List route Dining_1_
Big A Burger
====> Found a value for target column --> Big A Burger
```

Figure 9.34: Inner workings of the JSON flatten function

Now, we have successfully flattened the data into individual columns for better analysis. However, the list items are split into multiple columns in the same table, which could create wide column tables and sparse data, which is not good for querying the data. The better approach would be to split the data from the list into a separate table with foreign key relationships.

In order to achieve this, we will change our flatten function further. The following is a summary of changes made to the flatten function:

- Add logic to handle the primary key for the dataset so that the primary key can be stored in the child table to join them together.
- We will change the flattening logic for list objects to create a different DataFrame. The logic also will populate the primary key iterator column to identify different values from the original list object.
- Now we will have more than one dictionary item returned from our function, so we will change the output of the function to return a list of dictionaries/list objects.

```
#For learning the working of the function.
def flatten_v2(x, name='',df_name="df",pk="Name",pk_val="",out={},out_dfs={}):
    print("Input -->",x, "--", df_name,"--",name)
    a=""
    df1={}
    df_list=[]
    if pk_val=="":
        pk_val=x[pk]
    if type(x) is dict:
        for a in x:
            print("___________________________")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_',"df",pk_val=pk_val)
    elif type(x) is list:
        i = 0
        for a in x:
            df1={}
            print("***************************")
            print("List route " + name + str(i) + '_')
            df_name=name[:-1]
            df1[name[:-1]]=a
            df1[name[:-1]+"_pk"]=i
            df1[pk]=pk_val
            df_list.append(df1)
            print("Output in list-->",df_list)
            i += 1
        out_dfs[df_name]=df_list
        return out_dfs
    else:
        print("====> Found a value for target column --> "+ x)
        out[name[:-1]] = x
    out_dfs[df_name]=out
    print("Output-->",out)
    return out_dfs
```

Figure 9.35: Updated flatten function to split list objects into another DataFrame with a primary foreign key relationship
