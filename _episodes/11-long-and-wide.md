---
title: "Wide and long data formats"
teaching: 20
exercises: 0
questions:
- "What are long and Wide formats?"
- "Why would I want to change between them?"
objectives:
- "Explain difference between long and wide formats and why each might be used"
- "Illustrate how to change between formats using the `melt()` and `pivot()` methods"
keypoints:
- "The `melt()` method can be used to change from wide to long format"
- "The `pivot()` method can be used to change from the long to wide format"
- "Aggregations are best done from data in the long format."
---

## Wide and long data formats

In the Farms table the F14_items_owned column contains a list of the items owned by each Farmer. The Items_wide.csv filehas this same information but now each type of item has its own column.
This format is more in keeping with the `Tidy` principles whereby each variable has its own column. The downside is that it is not much use for aggregation type functions.

What we would like to do is to create a row for each type of item within each Farm along with an indication of whether or not the Farm has that item.
This process is referred to as converting from 'wide' to 'long'


## From 'wide' to 'long'

In the Items_wide.csv file there are 20 columns, The 'Id' column, the original 'F14_items_owned' column and then 18 columns one for each possible item. 
The values in thes columns are 1 or 0 to indicate whether the item is owned or not.

When we do the conversion we need to keep the 'Id' column, we are not really interested in the original 'F14_items_owned' column so we 
will ignore it. The remaining 18 columns for the different Item types we want to `melt` into a single column using the original column name as the value. In addition we need a new column which will contain the original value from the Id/Item intersection.


~~~
## using 'Items_wide.csv'

df_Items_wide = pd.read_csv("Items_wide.csv")  # read the Iems_wide.csv file
item_list = df_Items_wide.columns[2:20]        # create a list of the last 18 column names
item_list                                      # check we have selected the right values
df_Items_wide.shape                            # check the shape

df_Items_long = pd.melt(df_Items_wide,  id_vars = ['Id'], value_vars = item_list ) # perform the melt
df_Items_long.to_csv("df_Items_long")                                              # optional write the output to a csv file
df_Items_long.head()                                                               # check the output
df_Items_long.shape                                                                # check the shape

# by default the new columns created will be called 'variable' which contains the names of the items
# and 'value' which is the value of that 'item' for that 'Id'. 
# We can rename these columns to something more meaningful 

df_Items_long.columns = ['Id','Item_owned','Yes_No']
df_Items_long
~~~
{: .language-python}

We now have a Dataframe that we can `groupby`.

We want to `groupby` the `Item_owned` and then sum the `Yes_No`.

~~~
df_Items_long.groupby('Item_owned')['Yes_No'].sum()

~~~
{: .language-python}

~~~
Item_owned
bicycle         201
car               5
computer          4
cow_cart         30
cow_plough       92
electricity      33
fridge           18
lorry             7
mobile_phone    214
motorcyle        52
radio           202
sofa_set         20
solar_panel     136
solar_torch     122
sterio           32
table           125
television       60
tractor           2
~~~
{: output}

## From Long to Wide

The process can be reversed by using the `pivot()` method.
Here we need to indicate which column (or columns) remain fixed (this will become an index in the new Dataframe), 
which column contains the values which are to become column names and which column contains the values for the columns.

In our case we want to use the `Id` column as the fixed column, the `Item_owned` column contains the column names and the `Yes_No` column contains the values.

~~~
df_Items_wide_again = df_Items_long.pivot(index = 'Id', columns = 'Item_owned', values = 'Yes_No')
df_Items_wide_again.to_csv("Items_wide_again.csv")
~~~
{: .language-python}

We can change our `Id` index back to an ordinary column with

~~~
df_Items_wide_again.reset_index(level=0, inplace=True)
~~~
{: .language-python}

