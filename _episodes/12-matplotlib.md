---
title: "Data visualisation using Matplotlib"
teaching: 25
exercises: 25
questions:
- "How can I create simple visualisations of my data?"
objectives:
- "Import pyplot from the matplotlib library"
- "Create simple plots using pyplot"
keypoints:
- "Graphs can be drawn directly from pandas, but it still uses matplotlib"
- "Different graph types have different data requirements"
- "Graphs are created from a variety of discrete components placed on a 'canvas', you don't have to use them all"
- "Plotting multiple graphs on a single 'canvas' is possible"
---

## Matplotlib

Matplotlib is a Python graphical library that can be used to produce a variety of different graph types.

The pandas library contains very tight integration with matplotlib. There are functions in pandas that automatically call matplotlib functions to produce graphs.

Although we are using Matplotlib in this episode, pandas can make use of several other graphical libraries available from within Python such as ggplot2 and seaborn.

## Importing matplotlib

The matplotlib library can be imported using any of the import techniques we have seen. As `pandas` is generally imported with `import panas as pd`, you will find that `matplotlib` is most commonly imported with `import matplotlib as plt` where 'plt' is the alias.

In addition to importing the library, in a Jupyter notebook environment we need to tell Jupyter that when we produce a graph we want it to be display the graph in a cell in the notebook just like any other results. To do this we use the `%matplotlib inline` directive.  

If you forget to do this, you will have to add `plt.show()` to see the graphs.

~~~
import matplotlib.pyplot as plt
%matplotlib inline
~~~
{: .language-python}

## Numpy

Numpy is another Python library. It is used for multi-dimensional array processing. 
In our case we just want to use it for its useful random number generation functions 
which we will use to create some fake data to demonstrate some of the graphing functions of matplotlib.

We will use the alias `np`, following convention.

~~~
import numpy as np
~~~
{: .language-python}

## Bar charts

~~~
np.random.rand(20)
~~~
{: .language-python}

will generate 20 random numbers between 0 and 1.

We are using these to create a pandas Series of values.

## Bar charts

A bar chart only needs a single set of values. Each 'bar' represents the value from the Series of values.
A pandas Series (and a Dataframe) has a method called 'plot'. We only need to tell plot what kind of graph we want.

The 'x' axis represents the index values of the Series


~~~
import numpy as np
import pandas as pd

np.random.seed(12345)            # set a seed value to ensure reproducibility of the plots
s = pd.Series(np.random.rand(20) )
#s
# plot the bar chart
s.plot(kind='bar')
~~~
{: .language-python}

Internally the pandas 'plot' method has called the 'bar' method of matplotlib and provided a set of parameters, including the pandas.Series s to generate the graph.

We can use matplotlib directly to produce a similar graph. In this case we need to pass two parameters, the number of bars we need and the pandas Series holding the values.

We also have to explicitly call the `show()` function to produce the graph.

~~~
plt.bar(range(len(s)), s)
plt.show()
~~~
{: .language-python}


## Histograms

We can plot histograms in a similar way, directly from pandas and also from Matplotlib

The pandas way

~~~
s = pd.Series(np.random.rand(20))
# plot the bar chart
s.plot(kind='hist')
~~~
{: .language-python}

and the matplotlib way

~~~
plt.hist(s)
plt.show()
~~~
{: .language-python}

For the Histogram, each data point is allocated to 1 of 10 (by default) equal 'bins' of equal size (range of numbers) which are indicated 
along the x axis and the number of points (frequency) is shown on the y axis.

In this case the graphs are almost identical. The only difference being in the first graph the y axis has a label 'Frequency' associated with it.

We can fix this with a call to the `ylabel` function

~~~
plt.ylabel('Frequency')
plt.hist(s)
plt.show()
~~~
{: .language-python}

You can also specifiy your own set of 'bins'

~~~
plt.ylabel('Frequency')
plt.hist(s, bins=[0, 0.1, 0.2, 0.3, 0.4, 0.5, 1])
plt.show()
~~~
{: .language- python}

In general most graphs can be broken down into a series of elements which, although typically related in some way, can all exist independently of each other. This allows us to create the graph in a rather piecemeal fashion.

The labels (if any) on the x and y axis are independent of the data values being represented. The title and the legend are also independent objects within the overall graph.

In matplotlib you create the graph by providing values for all of the individual components you choose to include. When you are ready, you call the `show` function.

Using this same approach we can plot two sets of data on the same graph

We will use a scatter plot to demonstrate some of the available features.

For a scatter plot we need two sets of data points one for the x values
and the other for the y values.

## Scatter plot

The scatter plot requires the x and y coordinates of each of the points being plotted.
To provide this we will generate two series of random data one for the x coordinates and the other for the y coordinates

We will generate two sets of points and plot them on the same graph.

We will also add other common features like a title, a legend and labels on the x and y axis.

~~~
# Generate some date for 2 sets of points.
x1 = pd.Series(np.random.rand(20) - 0.5 )
y1 = pd.Series(np.random.rand(20) - 0.5 )

x2 = pd.Series(np.random.rand(20) + 0.5 )
y2 = pd.Series(np.random.rand(20) + 0.5 )


# Add some features
plt.title('Scatter Plot')
plt.ylabel('Range of y values')
plt.xlabel('Range of x values')

# plot the points in a scatter plot
plt.scatter(x1,y1, c='red', label='Red Range' )  # 'c' parameter is the colour and 'label' is the text for the legend
plt.scatter(x2,y2, c='blue', label='Blue Range')

# Add a legend
plt.legend( loc=4 )  # the locations 1,2,3 and 4 are top-right, top-left, bottom-left and bottom-right

# Show the graph with the two sets of points
plt.show()
~~~
{: .language-python}

In the call to the `scatter` method, the `label` parameter values are used by the _legend_.
The `c` or `color` parameter can be set to any color matplotlib recognises. Full details of the available colours are available in the [matplotlib](http://matplotlib.org/api/colors_api.html) website. The [markers](http://matplotlib.org/api/markers_api.html) section will tell you what markers you can use instead of the default 'dots'. There is also an `s` (size) parameter which allows you to change the size of the marker.

> ## Exercise
>
> In the scatterplot the s parameter determines the size of the dots. s can be a simple numeric value, say s=100, which will produce dots all of the same size. However you can pass a list of values (or a pandas Series) to provide sizes for the individual dots. This approach is very common as it allows us to provide an extra variable worth of information on the graph.
>
> 1. Modify the code we used for the scatter plot to include a size value for each of the points in the series being plotted.
> (The downside is that some of the smaller dots may be completely covered by the larger dots. To try and highlight when this has happened we can change the opacity of the dots.)
>
> 2. Find out which parameter controls the opacity of the dots ( clue - it is not called opacity), add it to you code and set it > to a reasonable value .
>
> > ## Solution
> >
> > ~~~
> > # Generate some data for 2 sets of points.
> > # and additional data for the sizes - suitably scaled
> > x1 = pd.Series(np.random.rand(20) - 0.5 )
> > y1 = pd.Series(np.random.rand(20) - 0.5 )
> > z1 = pd.Series(np.random.rand(20)*200 )
> >
> > x2 = pd.Series(np.random.rand(20) + 0.5 )
> > y2 = pd.Series(np.random.rand(20) + 0.5 )
> > z2 = pd.Series(np.random.rand(20)*200 )
> >
> > # Add some features
> > plt.title('Scatter Plot')
> > plt.ylabel('Range of y values')
> > plt.xlabel('Range of x values')
> >
> > # plot the points in a scatter plot
> > plt.scatter(x1,y1, c='red', label='Red Range', s=z1, alpha=0.5 )  # 's' parameter is the dot size
> > plt.scatter(x2,y2, c='blue', label='Blue Range', s=z2, alpha=0.5) # 'alpha' is the opacity
> >
> > plt.legend( loc=4 )
> > plt.show()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Exercise
>
> Create a scatterplot from the the 'A11_years_farm' and the 'B16_years_liv' columns from the Farms.csv dataset.
> Provide suitable titles. Interpret what you see in the graph.
>
> > ## Solution
> >
> > ~~~
> > # read the file into a dataset
> > df_Farms = pd.read_csv("Farms.csv")
> > 
> > # set up the x and y data - we could have just used them directly in the 'plt.scatter()' line
> > x = df_Farms.A11_years_farm
> > y = df_Farms.B16_years_liv
> > 
> > # Add some features
> > plt.title('Scatter Plot')
> > plt.ylabel('Years in area')
> > plt.xlabel('Years on Farm')
> > 
> > # plot the points in a scatter plot
> > plt.scatter(x,y)  
> > 
> > # Show the graph 
> > plt.show()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Boxplot

A boxplot provides a simple representation of a variety of statistical qualities of a single set of data values.

![box_plot](../fig/vis_boxplot_01.png)

~~~
x = pd.Series(np.random.standard_normal(256))

# Show a boxplot of the data
plt.boxplot(x)
plt.show()
~~~
{: .language-python}

A common use of the boxplot is to compare the statistical variations across a set of variables.

The variables can be an independent series or columns of a Dataframe.

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE')) # creating a Dataframe directly with pandas
plt.boxplot(df.A, labels = 'A')
plt.show()
~~~
{: .language-python}

> ## Exercise
>
> Can you change the code above so that columns `A` , `C` and `D` are all displayed on the same graph?
>
> > ## Solution
> >
> > ~~~
> > df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
> > plt.boxplot([df.A, df.C, df.D], labels = ['A', 'C', 'D'])
> > plt.show()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

The boxplot function cannot accept a whole Dataframe. The code

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
plt.boxplot(df)
plt.show()
~~~
{: .language-python}

will not produce the hoped for results.

However we can use the pandas plot method

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
df.plot(kind = 'box', return_type='axes') # the return_type='axes' is only needed for forward compatibility
~~~
{: .language-python}

We can add a title to the above by adding the `title` parameter. However there are no parameters for adding the axis labels.
To add labels we can use matplotlib directly.

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
df.plot(kind = 'box', return_type='axes')

plt.title('Box Plot')
plt.xlabel('xlabel')
plt.ylabel('ylabel')
plt.show()
~~~
{: .language-python}

## Saving a graph

If you wish to save your graph as an image you can do so using the `savefig()` function. The image can be saved as a pdf, jpg or png file by changing the file extension.

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
df.plot(kind = 'box', return_type='axes')

plt.title('Box Plot')
plt.xlabel('xlabel')
plt.ylabel('ylabel')
#plt.show()
plt.savefig('boxplot_from_df.pdf')
~~~
{: .language-python}

> ## Exercise
>
> Read the 'Plots.csv' file into a dataframe and create a boxplot of all of the numeric cariables excluding the 'Id' column
>
> > ## Solution
> >
> > ~~~
> > # Attempt 1
> > df = pd.read_csv("Plots.csv")
> > df.plot(kind = 'box')
> > ~~~
> > {: .language-python}
> > 
> > This , plot the whole dataframe, attempt included the 'Id' column and you can see the effect it has on the scale. 
> > Also the text on the X axis is unreadable because of the length of the column names. This hides the fact
> > that the non-numeric column (D03_unit_land) is automatically omitted for us.
> > 
> > ~~~
> > # Attempt 2
> > df = pd.read_csv("Plots.csv")
> > df[df.columns[1:7]].plot(kind = 'box')
> > plt.xticks(rotation=90)
> > ~~~
> > 
> > In this attempt we have specified the columns that we wish to plot explicitly by using 'df.columns' and 
> > indicating that we want the columns with indexes 1 through 6 inclusive. Index 0 is the 'Id' column.
> > 
> > The `plt.xticks(rotation=90)` line rotates the X-axis text to make it readable.
> > 
> > ~~~
> > # Attempt 3
> > df = pd.read_csv("Plots.csv", usecols=range(1,7))
> > df.plot(kind = 'box', return_type='axes')
> > plt.xticks(rotation=90)
> > ~~~
> > 
> > This also works, but is perhaps simpler in that we use the `usecols` parameter when we read in 
> > the file to avoiud the 'Id' column altogether.
> > {: .language-python}
> {: .solution}
{: .challenge}
