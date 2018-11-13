---
title: "Reading data from a file using Pandas"
teaching: 15
exercises: 5
questions:
- "What is Pandas?"
- "How do I read files using Pandas?"
- "What is the difference between reading files using Pandas and other methods of reading files?"

objectives:
- "Explain what a module is and how they are used in Python"
- "Describe what the Python Data Analysis Library (pandas) is"
- "Load the Python Data Analysis Library (pandas)"
- "Use read_csv to read tabular data into Python"

keypoints:
- "pandas is a Python library containing functions and data structures to assist in data analysis"
- "pandas data structures are the Series (like a vector) and the Dataframe (like a table)"
- "the pandas `read_csv` function allows you to read an entire `csv` file into a Dataframe"
---

## What is Pandas?

pandas is a Python library containing a set of functions and specialised data structures that have been designed to help Python programmers to perform data analysis tasks in a structured way.

Most of the things that pandas can do can be done with basic Python, but the collected set of pandas functions and data structure makes the data analysis tasks more consistent in terms of syntax and therefore aids readabilty.

Particular features of pandas that we will be looking at over this and the next couple of episodes include:


* Reading data stored in CSV files (other file formats can be read as well)
* Slicing and subsetting data in Dataframes (tables!)
* Dealing with missing data
* Reshaping data (long -> wide,  wide -> long)
* Inserting and deleting columns from data structures
* Aggregating data using data grouping facilities using the split-apply-combine paradigm
* Joining of datasets (after they have been loaded into Dataframes)


If you are wondering why I write pandas with a lower case 'p' it is because it is the name of the package and Python is case sensitive.


## Importing the pandas library

Importing the pandas library is done in exactly the same way as for any other library. In almost all examples of Python code using the pandas library, it will have been imported and given an alias of `pd`. We will follow the same convention.


~~~
import pandas as pd
~~~
{: .language-python}

## Pandas data structures

There are two main data structure used by pandas, they are the Series and the Dataframe. The Series equates in general to a vector or a list. The Dataframe is equivalent to a table. Each column in a pandas Dataframe is a pandas Series data structure.

We will mainly be looking at the Dataframe.

We can easily create a Pandas Dataframe by reading a .csv file

## Reading a csv file

When we read a csv dataset in base Python we did so by opening the dataset, reading and processing a record at a time and then closing the dataset after we had read the last record. Reading datasets in this way is slow and places all of the responsibility for extracting individual data items of information from the records on the programmer.

The main advantage of this approach, however, is that you only have to store one dataset record in memory at a time. This means that if you have the time, you can process datasets of any size.

In Pandas, csv files are read as complete datasets. You do not have to explicitly open and close the dataset. All of the dataset records are assembled into a Dataframe. If your dataset has column headers in the first record then these can be used as the Dataframe column names. You can explicitly state this in the parameters to the call, but pandas is usually able to infer that there ia a header row and use it automatically.


We are going to read in our Farms.csv file and have a look at some of the data using a `print` statement. 
Although this is a tab delimited file we will still use the pandas `read_csv` method, but we will explicitly tell the method that the separator is the tab character and 
not a comma which is the default.

~~~
df_Farms = pd.read_csv("Farms.csv")
print(df_Farms)
~~~
{: .language-python}

> ## Exercise
>
> The 'pd.read_csv' method can also be used to read a 'tab' delimited file as well.
> You do this by adding an extra parameter to say what character you want to use as the seperator. 
> For a 'tab' seperated file we would add `sep='\t'` to the `pd.read_csv` method. Notice that we use `\t` to denote a tab character.
> Try including this in the previous bit of code and see what affect it has.
>
> > ## Solution
> >
> > ~~~
> > df_Farms_oops = pd.read_csv("Farms.csv", sep='\t')
> > print(df_Farms_oops.shape)
> > print(df_Farms_oops)
> > ~~~
> > {: .language-python}
> >
> > The `pd.read_csv` method allows you to read a text file using whatever seperator is necessary. But you must make sure you get it right. By default, i.,e. you 
> > don't provide a seperator character, it will assume a comma.
> >
> >
> {: .solution}
{: .challenge}

##  Getting information about a Dataframe

You can find out the type of the variable `df_SN7577` by using the `type` function.

~~~
print(type(df_Farms))
~~~
{: .language-python}

~~~
<class 'pandas.core.frame.DataFrame'>
~~~
{: .output}

You can see the contents by simply entering the variable name. 

~~~
df_Farms
~~~
{: .language-python}

You can see from the output that it is a tabular format. 
The column names have been taken from the first record of the file. On the left hand side is a column with no name. 
The entries here have been provided by pandas and act as an index to reference the individual rows of the Dataframe.

The `pd.read_csv()` method has an `index_col` parameter which you can use to indicate which of the columns in the file you wish to use as the index instead. The values in the column have to be unique.
For the Farms.csv dataset we could use the `Id` column as we know that it has unique values but as we have no cause to refer to the individual records by theirId we will not do this.

Another thing to notice about the display is that, like the `print` display, it is truncated. By default you will see the first and last 30 rows. For the columns you will always get the first few columns and typically the last few depending on display space.

Similar information can be obtained with `df_SN7577.head()` But here you are only returned the first 5 rows by default.

~~~
df_Farms.head()
~~~
{: .language-python}

> ## Exercise
>
> 1. As well as the `head()` method there is a `tail()` method. What do you think it does? Try it.
> 2. Both methods accept a single numeric parameter. What do you think it does? Try it.
> 
{: .challenge}

You can obtain other basic information about your Dataframe of data with:

~~~
# How many rows?
print(len(df_Farms))
# How many rows and columns - returned as a tuple
print(df_Farms.shape)
#How many 'cells' in the table
print(df_Farms.size)
# What are the column names
print(df_Farms.columns)
# what are the data types of the columns?
print(df_Farms.dtypes)
~~~
{: .language-python}

~~~
350
(350, 61)
21350
Index(['Id', 'Country', 'A01_interview_date', 'A03_quest_no', 'A04_start',
       'A05_end', 'A06_province', 'A07_district', 'A08_ward', 'A09_village',
       'A11_years_farm', 'A12_agr_assoc', 'B_no_membrs', '_members_count',
       'B11_remittance_money', 'B16_years_liv', 'B17_parents_liv',
       'B18_sp_parents_liv', 'B19_grand_liv', 'B20_sp_grand_liv',
       'C01_respondent_roof_type', 'C02_respondent_wall_type',
       'C03_respondent_floor_type', 'C04_window_type',
       'C05_buildings_in_compound', 'C06_rooms', 'C07_other_buildings',
       'D_no_plots', 'D_plots_count', 'E01_water_use', 'E_no_group_count',
       'E_yes_group_count', 'E17_no_enough_water', 'E18_months_no_water',
       'E19_period_use', 'E20_exper_other', 'E21_other_meth', 'E22_res_change',
       'E23_memb_assoc', 'E24_resp_assoc', 'E25_fees_water',
       'E26_affect_conflicts', 'F04_need_money', 'F05_money_source',
       'F05_money_source_other', 'F08_emply_lab', 'F09_du_labour',
       'F10_liv_owned', 'F10_liv_owned_other', 'F_liv_count', 'F12_poultry',
       'F13_du_look_aftr_cows', 'F14_items_owned', 'G01_no_meals',
       'G02_months_lack_food', 'G03_no_food_mitigation', 'gps:Latitude',
       'gps:Longitude', 'gps:Altitude', 'gps:Accuracy', 'instanceID'],
      dtype='object')
Id                             int64
Country                       object
A01_interview_date            object
A03_quest_no                   int64
A04_start                     object
...
Length: 61, dtype: object
~~~
{: .output}

> ## Exercise
>
> When we asked for the column names we got a `list` containing all of them. Can you write a small piece of code which will return all of the values
> one per line.
>
> > ## Solution
> >
> > ~~~
> > for name in df_Farms.columns:
> >     print(name)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
