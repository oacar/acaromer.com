I have been using tidyverse functions with dplyr or tidyr for data
manipulation mainly. There are times where I had to use Python due to
need for a specific package or collaboration with people using only
Python, thus needed to use Pandas for similar purposes. However, I am in
no way a Python expert and every time I needed to look up a lot of basic
things and end up just going back and forth between Python and R. Here,
I wanted to create a reference page with examples for myself and people
who are interested in same things to use Pandas and Tidyverse functions
for data manipulation purposes.

First to use python in R markdown, I use RStudio and reticulate

    library(reticulate)
    use_condaenv("r-reticulate")

Then let’s load the libraries and get the data, I’ll use iris dataset
that’s available by default in R:

### R

    library(dplyr)
    data("iris")
    #iris

### Python

You can get the same data from seaborn-data link.

    import pandas as pd
    iris = pd.read_csv('https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv')
    iris

    ##      sepal_length  sepal_width  petal_length  petal_width    species
    ## 0             5.1          3.5           1.4          0.2     setosa
    ## 1             4.9          3.0           1.4          0.2     setosa
    ## 2             4.7          3.2           1.3          0.2     setosa
    ## 3             4.6          3.1           1.5          0.2     setosa
    ## 4             5.0          3.6           1.4          0.2     setosa
    ## ..            ...          ...           ...          ...        ...
    ## 145           6.7          3.0           5.2          2.3  virginica
    ## 146           6.3          2.5           5.0          1.9  virginica
    ## 147           6.5          3.0           5.2          2.0  virginica
    ## 148           6.2          3.4           5.4          2.3  virginica
    ## 149           5.9          3.0           5.1          1.8  virginica
    ## 
    ## [150 rows x 5 columns]

Tidyverse functions are usually self-explained. In order to get rows
with a condition there is a `filter` function, to extract a column from
data frame there is `select`. `mutate` is used for adding new columns to
data frame based on values on other columns or as I use most of time
with external `ifelse` statements. Let’s see how these functions and
uses are in both languages. To actually compare functions and to not
confuse people who are not familiar with pipes in tidyverse, I will omit
using pipes and use data frames as arguments to functions.

Filter
------

Let’s take the rows with Sepal Length &gt; 5 condition.

### R

    iris_sepal_long <- filter(iris, Sepal.Length>5)
    head(iris_sepal_long)

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          5.4         3.9          1.7         0.4  setosa
    ## 3          5.4         3.7          1.5         0.2  setosa
    ## 4          5.8         4.0          1.2         0.2  setosa
    ## 5          5.7         4.4          1.5         0.4  setosa
    ## 6          5.4         3.9          1.3         0.4  setosa

### Python

    iris_sepal_long = iris[iris.sepal_length>5]
    iris_sepal_long

    ##      sepal_length  sepal_width  petal_length  petal_width    species
    ## 0             5.1          3.5           1.4          0.2     setosa
    ## 5             5.4          3.9           1.7          0.4     setosa
    ## 10            5.4          3.7           1.5          0.2     setosa
    ## 14            5.8          4.0           1.2          0.2     setosa
    ## 15            5.7          4.4           1.5          0.4     setosa
    ## ..            ...          ...           ...          ...        ...
    ## 145           6.7          3.0           5.2          2.3  virginica
    ## 146           6.3          2.5           5.0          1.9  virginica
    ## 147           6.5          3.0           5.2          2.0  virginica
    ## 148           6.2          3.4           5.4          2.3  virginica
    ## 149           5.9          3.0           5.1          1.8  virginica
    ## 
    ## [118 rows x 5 columns]

Any logical statements combined with logical `OR` or `AND` signs can be
used when filtering data frame. I’ll show with python but same applies
to R as well. For example we can select rows with Sepal length &gt; 5
and Sepal width &lt; 4 in `setosa` species in the following way:

    iris_sepal_filter = iris[(iris.sepal_length>5)&(iris.sepal_width<4)&(iris.species=='setosa')]
    iris_sepal_filter

    ##     sepal_length  sepal_width  petal_length  petal_width species
    ## 0            5.1          3.5           1.4          0.2  setosa
    ## 5            5.4          3.9           1.7          0.4  setosa
    ## 10           5.4          3.7           1.5          0.2  setosa
    ## 16           5.4          3.9           1.3          0.4  setosa
    ## 17           5.1          3.5           1.4          0.3  setosa
    ## 18           5.7          3.8           1.7          0.3  setosa
    ## 19           5.1          3.8           1.5          0.3  setosa
    ## 20           5.4          3.4           1.7          0.2  setosa
    ## 21           5.1          3.7           1.5          0.4  setosa
    ## 23           5.1          3.3           1.7          0.5  setosa
    ## 27           5.2          3.5           1.5          0.2  setosa
    ## 28           5.2          3.4           1.4          0.2  setosa
    ## 31           5.4          3.4           1.5          0.4  setosa
    ## 36           5.5          3.5           1.3          0.2  setosa
    ## 39           5.1          3.4           1.5          0.2  setosa
    ## 44           5.1          3.8           1.9          0.4  setosa
    ## 46           5.1          3.8           1.6          0.2  setosa
    ## 48           5.3          3.7           1.5          0.2  setosa

Mutate
------

Let’s make another data frame with a column of Sepal Length to Sepal
Width ratio. While `mutate` is the function in R, `assign` will do the
same thing in Python.

### R

    #iris$newcolumn <- iris$Sepal.Length/iris$Sepal.Width
    iris_sepal_length_over_width <- mutate(iris,sepal_length_over_width = Sepal.Length / Sepal.Width)
    head(iris_sepal_length_over_width)

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa
    ##   sepal_length_over_width
    ## 1                1.457143
    ## 2                1.633333
    ## 3                1.468750
    ## 4                1.483871
    ## 5                1.388889
    ## 6                1.384615

### Python

    iris_sepal_length_over_width = iris.assign(sepal_length_over_width = iris.sepal_length / iris.sepal_width)
    iris_sepal_length_over_width

    ##      sepal_length  sepal_width  ...    species  sepal_length_over_width
    ## 0             5.1          3.5  ...     setosa                 1.457143
    ## 1             4.9          3.0  ...     setosa                 1.633333
    ## 2             4.7          3.2  ...     setosa                 1.468750
    ## 3             4.6          3.1  ...     setosa                 1.483871
    ## 4             5.0          3.6  ...     setosa                 1.388889
    ## ..            ...          ...  ...        ...                      ...
    ## 145           6.7          3.0  ...  virginica                 2.233333
    ## 146           6.3          2.5  ...  virginica                 2.520000
    ## 147           6.5          3.0  ...  virginica                 2.166667
    ## 148           6.2          3.4  ...  virginica                 1.823529
    ## 149           5.9          3.0  ...  virginica                 1.966667
    ## 
    ## [150 rows x 6 columns]

For Python, following is the equivalent of assign:

    iris['sepal_length_over_width'] = iris.sepal_length / iris.sepal_width
    iris

    ##      sepal_length  sepal_width  ...    species  sepal_length_over_width
    ## 0             5.1          3.5  ...     setosa                 1.457143
    ## 1             4.9          3.0  ...     setosa                 1.633333
    ## 2             4.7          3.2  ...     setosa                 1.468750
    ## 3             4.6          3.1  ...     setosa                 1.483871
    ## 4             5.0          3.6  ...     setosa                 1.388889
    ## ..            ...          ...  ...        ...                      ...
    ## 145           6.7          3.0  ...  virginica                 2.233333
    ## 146           6.3          2.5  ...  virginica                 2.520000
    ## 147           6.5          3.0  ...  virginica                 2.166667
    ## 148           6.2          3.4  ...  virginica                 1.823529
    ## 149           5.9          3.0  ...  virginica                 1.966667
    ## 
    ## [150 rows x 6 columns]

Select
------

This is one of the most important functions when dealing with data
frames. You don’t always need all the data in a data frame but only some
columns.

Let’s select columns related with Sepal data.

### R

    sepal_data <- select(iris,Sepal.Length,Sepal.Width)
    head(sepal_data)

    ##   Sepal.Length Sepal.Width
    ## 1          5.1         3.5
    ## 2          4.9         3.0
    ## 3          4.7         3.2
    ## 4          4.6         3.1
    ## 5          5.0         3.6
    ## 6          5.4         3.9

### Python

    sepal_data = iris[['sepal_length','sepal_width']]
    sepal_data

    ##      sepal_length  sepal_width
    ## 0             5.1          3.5
    ## 1             4.9          3.0
    ## 2             4.7          3.2
    ## 3             4.6          3.1
    ## 4             5.0          3.6
    ## ..            ...          ...
    ## 145           6.7          3.0
    ## 146           6.3          2.5
    ## 147           6.5          3.0
    ## 148           6.2          3.4
    ## 149           5.9          3.0
    ## 
    ## [150 rows x 2 columns]

However one thing I do a lot when using `select` getting a column and
turn it into a vector because `select` returns a data frame with a
single column, for quick plotting with base R functions. To do this in R
you need to use `pull` function after selecting the data. Then you will
have a vector of the selected column which can be directly used in
plotting functions like `plot` or `hist`

### R

    sepal_width_vector <- pull(select(iris,Sepal.Width))
    head(sepal_width_vector)

    ## [1] 3.5 3.0 3.2 3.1 3.6 3.9

    hist(sepal_width_vector)

![](2019-08-05-comparison-of-data-packages_files/figure-markdown_strict/unnamed-chunk-13-1.png)

### Python

I think there would be different solution to this in Python way but to
directly replicate what I did above in R you can call tolist() function
on the column data.

    sepal_width_vec = iris.sepal_width.tolist()
    sepal_width_vec

    ## [3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.4, 2.9, 3.1, 3.7, 3.4, 3.0, 3.0, 4.0, 4.4, 3.9, 3.5, 3.8, 3.8, 3.4, 3.7, 3.6, 3.3, 3.4, 3.0, 3.4, 3.5, 3.4, 3.2, 3.1, 3.4, 4.1, 4.2, 3.1, 3.2, 3.5, 3.6, 3.0, 3.4, 3.5, 2.3, 3.2, 3.5, 3.8, 3.0, 3.8, 3.2, 3.7, 3.3, 3.2, 3.2, 3.1, 2.3, 2.8, 2.8, 3.3, 2.4, 2.9, 2.7, 2.0, 3.0, 2.2, 2.9, 2.9, 3.1, 3.0, 2.7, 2.2, 2.5, 3.2, 2.8, 2.5, 2.8, 2.9, 3.0, 2.8, 3.0, 2.9, 2.6, 2.4, 2.4, 2.7, 2.7, 3.0, 3.4, 3.1, 2.3, 3.0, 2.5, 2.6, 3.0, 2.6, 2.3, 2.7, 3.0, 2.9, 2.9, 2.5, 2.8, 3.3, 2.7, 3.0, 2.9, 3.0, 3.0, 2.5, 2.9, 2.5, 3.6, 3.2, 2.7, 3.0, 2.5, 2.8, 3.2, 3.0, 3.8, 2.6, 2.2, 3.2, 2.8, 2.8, 2.7, 3.3, 3.2, 2.8, 3.0, 2.8, 3.0, 2.8, 3.8, 2.8, 2.8, 2.6, 3.0, 3.4, 3.1, 3.0, 3.1, 3.1, 3.1, 2.7, 3.2, 3.3, 3.0, 2.5, 3.0, 3.4, 3.0]

    import seaborn as sns

    sns.set_style('darkgrid')
    sns.distplot(sepal_width_vec)

![](2019-08-05-comparison-of-data-packages_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Conclusions
-----------

Here I compared the mostly used functions in data science when dealing
with data frames in R using dplyr functions and in Python using Pandas.
These are only single ways of doing these things and R has multiple
different ways of doing same things due to difference in base R data
frame functions and packages like `dplyr` or `data.table`. I feel
`tidyverse` way of doing things are intuitive and easy to read
especially using pipes.

As far as I know Pandas is the mostly used package for working with data
frames in Python. I am sure there are other packages but I never need to
work with them or encountered. Pandas website also has a [reference
page](https://pandas.pydata.org/pandas-docs/stable/getting_started/comparison/comparison_with_r.html)
for R and Pandas function comparisons.

<!-- ## Sorting -->
<!-- ```{r} -->
<!-- arrange(iris) -->
<!-- ``` -->
<!-- ## Summarizing data with groups -->
