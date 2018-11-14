---
title: "Joining Pandas Dataframes"
teaching: 25
exercises: 10
questions:
- "How can I join two Dataframes with a common key?"
objectives:
- "Understand why we would want to join Dataframes"
- "Know what is needed for a join to be possible"
- "Understand the different types of joins"
- "Understand what the joined results tell us about our data"
keypoints:
- "You can join pandas Dataframes in much the same way as you join tables in SQL"
- "The `concat()` function can be used to concatenate two Dataframes by adding the rows of one to the other."
- "`concat()` can also combine Dataframes by columns but the `merge()` function is the preferred way"
- "The `merge()` function is equivalent to the SQL JOIN clause. 'left', 'right' and 'inner' joins are all possible."
---

## Joining Dataframes

### Why do we want to do this?


There are many occasions when we have related data spread across multiple files.

The data can be related to each other in different ways. How they are related and how completely we can join the data from the datasets will vary.

In this episode we will consider different scenarios and show how we might join the data. We will use a set of csv files based on the Farms.csv
file and in all cases the first step will be to read the datasets into a pandas Dataframe from where we will do the joining. 
The number of columns in the csv files we are using are have been reduced to make the displays more manageable.

### Scenario 1 - Two data sets containing the same columns but different **rows** of data

Here we want to add the rows from one Dataframe to the rows of the other Dataframe. In order to do this we can use the `concat()` function.

~~~
import pandas as pd

df_Farms_a = pd.read_csv("Farms_a.csv")
df_Farms_b = pd.read_csv("Farms_b.csv")
~~~
{: .language-python}

Have a quick look at what these Dataframes look like with

~~~
print(df_Farms_a)
print(df_Farms_b)
~~~
{: .language-python}


~~~

   Id A09_village  A11_years_farm  B_no_membrs C01_respondent_roof_type  \
0   1         God              11            3                    grass   
1   2         God               2            7                    grass   
2   3         God              40           10            mabatisloping   
3   4         God               6            7            mabatisloping   
4   5         God              18            7                    grass   
5   6         God               3            3                    grass   
6   7         God              20            6                    grass   

  C02_respondent_wall_type C03_respondent_floor_type  
0                  muddaub                     earth  
1                  muddaub                     earth  
2              burntbricks                    cement  
3              burntbricks                     earth  
4              burntbricks                     earth  
5                  muddaub                     earth  
6                  muddaub                     earth  
   Id A09_village  A11_years_farm  B_no_membrs C01_respondent_roof_type  \
0   8    Chirodzo              16           12            mabatisloping   
1   9    Chirodzo              16            8                    grass   
2  10    Chirodzo              22           12            mabatisloping   
3  11         God               6            6                    grass   
4  12         God              20            7            mabatisloping   

  C02_respondent_wall_type C03_respondent_floor_type  
0              burntbricks                    cement  
1              burntbricks                     earth  
2              burntbricks                    cement  
3                sunbricks                     earth  
4              burntbricks                    cement 
~~~
{: output}

The `concat()` function appends the rows from the two Dataframes to create the df_all_rows Dataframe. When you list this out you can see that all of the data rows are there, however there is a problem with the `index`.

~~~
df_all_rows = pd.concat([df_Farms_a, df_Farms_b])
df_all_rows
~~~
{: .language-python}


~~~

   Id A09_village  A11_years_farm  B_no_membrs C01_respondent_roof_type  \
0   1         God              11            3                    grass   
1   2         God               2            7                    grass   
2   3         God              40           10            mabatisloping   
3   4         God               6            7            mabatisloping   
4   5         God              18            7                    grass   
5   6         God               3            3                    grass   
6   7         God              20            6                    grass   
0   8    Chirodzo              16           12            mabatisloping   
1   9    Chirodzo              16            8                    grass   
2  10    Chirodzo              22           12            mabatisloping   
3  11         God               6            6                    grass   
4  12         God              20            7            mabatisloping 
~~~
{: output}

We didn't explicitly set an index for any of the Dataframes we have used. For `df_Farms_a` and `df_Farms_b` default indexes would have been created by pandas. 
When we concatenated the Dataframes the indexes were also concatenated resulting in duplicate entries.

This is really only a problem if you need to access a row by its index. We can fix the problem with the following code.

~~~
df_all_rows=df_all_rows.reset_index(drop=True)
df_all_rows
~~~
{: .language-python}

What if the columns in the Dataframes are not the same?

~~~
df_Farms_aa = pd.read_csv("Farms_aa.csv")
df_Farms_bb = pd.read_csv("Farms_bb.csv")
df_all_rows = pd.concat([df_Farms_aa, df_Farms_bb])
df_all_rows
~~~
{: .language-python}

In this case `df_Farms_aa` has no A09_village, A11_years_farm and B_no_membrs columns and `df_Farms_bb` has no 
A09_village, C01_respondent_roof_type, C02_respondent_wall_type and C03_respondent_floor_type columns. When they are concatenated, 
the resulting Dataframe has a column for all of these columns. For the rows corresponding to `df_Farms_aa` 
the values in the columns that it didn't have we see the `NaN` value. The same applies to missing columns for the `df_Farms_bb` rows.


### Scenario 2 - Adding the **columns** from one Dataframe to those of another Dataframe

~~~
df_Farms_c = pd.read_csv("Farms_c.csv")
df_Farms_d = pd.read_csv("Farms_d.csv")
df_all_cols = pd.concat([df_Farms_c, df_Farms_d], axis = 1)
df_all_cols
~~~
{: .language-python}

We use the `axis=1` parameter to indicate that it is the columns that need to be joined together. Notice that the `Id` column appears twice, 
because it was a column in each dataset. This is not particularly desirable, but also not necessarily a problem. 
However there are better ways of combining columns from two Dataframes which avoid this problem.

### Scenario 3 - Using merge to join columns

We can join columns from two Dataframes using the `merge()` function. This is similar to the SQL 'join' functionality.

A detailed discussion of different join types is given in the SQL lesson.

In order to `merge` the Dataframes we need to identify a column common to both of them.

~~~
df_cd = pd.merge(df_Farms_c, df_Farms_d, how='inner', on = 'Id')
df_cd
~~~
{: .language-python}


You specify the type of join you want using the `how` parameter. The default is the `inner` join which returns the columns from both tables where the 
`key` or common column values match in both Dataframes.

The possible values of the `how` parameter are shown in the picture below (taken from the Pandas documentation)

![pandas_join_types](../fig/pandas_join_types.png)

The different join types behave in the same way as they do in SQL. In Python/pandas, any missing values are shown as `NaN`


If there is only one column with the same name in each Dataframe, it will be assumed to be the one you want to join on. 
In this example the `Id` column, however leaving the join column to default in this way is not best practice. 
It is better to explicitly name the column using the `on` parameter.



In many circumstances, the column names that you wish to join on are not the same in both Dataframes, in which case you can use the `left_on` and 
`right_on` parameters to specify them separately.

~~~
df_cd = pd.merge(df_Farms_c, df_Farms_d, how='inner', left_on = 'Id', right_on = 'Id')
~~~
{: .language-python}

You specify the type of join you want using the `how` parameter. The default is the `inner` join which returns the columns from both tables where the 
`key` or common column values match in both Dataframes.




> ## Exercise
>
> We have three tables Farms, Plots and Crops. These tables are related to each other in that a Farm has Plots of land in which Crops are grown.
> 
> 1. Examine the contents of the Farms and Plots table and decide how to join them.
> 2. Perform the join
> 3. Compare the number of rows in the joined dataframe with the number of rows in the Plots table. How do you explainn the difference?
> 4. Use a `how='right' merge to highlight the discrepency
>
> > ## Solution
> >
> > ~~~
> > df_Farms = pd.read_csv("Farms.csv")
> > df_Plots = pd.read_csv("Plots.csv")
> > df_Farm_Plots = pd.merge(df_Farms, df_Plots, how='inner', left_on = 'Id', right_on = 'Id')
> > print(df_Plots.shape)
> > print(df_Farm_Plots.shape)
> > df_Farm_Plots_right = pd.merge(df_Farms, df_Plots, how='right', left_on = 'Id', right_on = 'Id')
> > print(df_Farm_Plots_right.shape)
> > df_Farm_Plots_right.tail()  # alternatively create a csv file from the dataframe and use Excel or spreadsheet or the Jupyter lab viewer
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}
