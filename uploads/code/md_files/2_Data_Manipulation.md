<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap">
</head>

<body>

<div style="background-color: #65BF6F; color: black; text-align: center; padding: 10px; font-family: 'Roboto', sans-serif; font-size: 24px">
    

<div style="text-align:center;">
    <img src="Jaki_Charrua_gatodelespacio.jpg" alt="Drawing" style="width:150px; border-radius:90%;"/>
    
    
</div> 


<body>


    
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://use.typekit.net/your-typekit-id.css">
</head>
<body>
    
    
<div style="background-color: #000000; color: white; text-align: center; padding: 10px; font-family: 'Proxima Nova', sans-serif; font-size: 24px;">
    Replication Lab  <br>
    GEC0319 | Time Series <br>
    
    
</div>



    
    
</body>
</html>

<h3 align="center" style="margin-top:20px">Tutorial 2: Data Manipulation with pandas</h3>
<br>

The first step in any data project is to get the data ready for analysis. In a traditional curriculum, the dataset was usually taken for granted and ready for analysis from the start. However, this no longer reflects the reality in industry: the trend towards [big data](http://www.forbes.com/sites/bernardmarr/2016/04/28/big-data-overload-most-companies-cant-deal-with-the-data-explosion/#52c807d83920) brought with it an increased demand for professionals that have the computational skills to work with possibly large, complex and unstructured data. It is [commonly stated](https://www.r-bloggers.com/the-real-prerequisite-for-machine-learning-isnt-math-its-data-analysis/) that up to 80% of a data scientist's job can be data preparation, exploratory data analysis (EDA), and visualisation. 

Common tasks include loading and inspecting the data, merging datasets, identifying and handling errors (data cleaning), handling missing values, etc.

In this lesson we explore the basics of how to use the [pandas](https://pandas.pydata.org/) package to work  with data in Python. Pandas is an industry-standard library that provides a highly flexible data structure called a dataframe, which makes data manipulation fast and easy. As a complement to this tutorial, the <a href="http://pandas.pydata.org/pandas-docs/stable/10min.html" target="_blank">10 minutes to pandas</a> tutorial in the official pandas documentation is also a useful starting reference.


<a href="#1.-Dataset">Dataset</a> <br>
<a href="#2.-Importing-data">Importing data</a> <br>
<a href="#3.-Inspecting-the-Data">Inspecting the data</a> <br>
<a href="#4.-Data-selection">Data selection</a> <br>
<a href="#5.-Conditional-data-selection">Conditional data selection</a> <br>
<a href="#6.-Assigning-new-values">Assigning new values</a> <br>
<a href="#7.-Descriptive-statistics">Descriptive statistics</a> <br>
<a href="#8.-Exporting-data">Exporting data</a> <br>

### 1. Dataset

This tutorial is based on the `Credit` dataset from the  <a href="http://www-bcf.usc.edu/~gareth/ISL/index.html" target="_blank">Introduction to Statistical Learning</a> texbook by James, Witten, Hastie and Tibshirani. It records the average credit card balace at end of the month for customers of a financial services company, as well as other individual characteristics such age, education, gender, marital status, number of cards, and credit rating.

We will be using `Credit` a few tutorials in this initial part of the course. Our objective will be to find a suitable model to predict the average credit card balance for different customers. The financial institution may be interested in this for multiple reasons: credit risk assessment, forecasting revenue, estimating the customer lifetime value (CLV), credit portfolio management, etc. 

**The dataset can be found &#128073;  [here](https://github.com/UCLSPP/datasets/blob/master/data/Credit.csv)
or [here](https://github.com/UCLSPP/datasets/blob/master/data/Credit.csv)**
### 2. Importing data

We start by loading the pandas package.


```python
import pandas as pd
```

Since the dataset is in the `csv` format, we use the pandas <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html" target="_blank">read_csv</a> function to import the file. The package will automatically read the column names and infer the variable types.  We assign the data variable to a variable called `data`, which will store the dataset as an object called a `DataFrame`.


```python
# We will always assume that the data file is in a subdirectory called "Data"
data = pd.read_csv('Data/Credit.csv')
type(data)
```




    pandas.core.frame.DataFrame



The data file needs to be in the correct place for this to work. You can run the `pwd` command to check which folder your notebook is running from. This folder needs to have a subfolder called `Data`, and the `Credit.csv` file needs to be there. If the data file was in the same folder as the notebook, the command would be  `pd.read_csv('Credit.csv')`.

*Note: To get help on any function or object, append a question mark to it and run the cell. For example, try running `pd.read_csv?`*.

Our present dataset is quite simple to work with, but others may require more complex calls to `read_csv`. Refer to the documentation to find the appropriate options for other situations that you may come across. 

Pandas also has specialised functions for reading other types of input files, such as Excel files. You can see a list of available functions <a href="http://pandas.pydata.org/pandas-docs/stable/io.html" target="_blank">here</a>. The pandas  <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_table.html#pandas.read_table" target="_blank">read table</a> function reads data stored as general delimited text files (for example, where the columns are separated by space rather than commas).  In practical business situations, you may often need to obtain data from a [relational database](https://en.wikipedia.org/wiki/Relational_database) rather than a flat file stored in your computer. You can read database tables and queries into a `DataFrame` by using the `read_sql_table`, `read_sql_query`, or `read_sql`  functions.  

### 3. Inspecting the data

A pandas `DataFrame` has some similarities to a spreadsheet. However, unlike a spreadsheet, you are not able to click through it or manually make modifications in the current setup. We do everything through coding. This may initially feel restrictive, but it is ultimately more efficient and scalable to work in this way. Some Python environments have GUIs (graphical user interfaces) that allow you to view and modify dataframes by clicking through an interface.  

The first thing that we'll want to do is to inspect the data. Two basic methods to do this are the `head` and `tail` methods. The head method displays the first rows of the data (by default five), while the tail displays the last rows. You can specify the number of rows within the parentheses. 

Running a cell with only the name of the `DataFrame` will provide a full view, but pandas limits number of rows and columns that can be shown by default. Check [here](http://stackoverflow.com/questions/19124601/is-there-a-way-to-pretty-print-the-entire-pandas-series-dataframe) if you want to change this setting.


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Obs</th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>148.924</td>
      <td>9504</td>
      <td>681</td>
      <td>3</td>
      <td>36</td>
      <td>11</td>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>964</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>55.882</td>
      <td>4897</td>
      <td>357</td>
      <td>2</td>
      <td>68</td>
      <td>16</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>331</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Obs</th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>395</th>
      <td>396</td>
      <td>12.096</td>
      <td>4100</td>
      <td>307</td>
      <td>3</td>
      <td>32</td>
      <td>13</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>560</td>
    </tr>
    <tr>
      <th>396</th>
      <td>397</td>
      <td>13.364</td>
      <td>3838</td>
      <td>296</td>
      <td>5</td>
      <td>65</td>
      <td>17</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>African American</td>
      <td>480</td>
    </tr>
    <tr>
      <th>397</th>
      <td>398</td>
      <td>57.872</td>
      <td>4171</td>
      <td>321</td>
      <td>5</td>
      <td>67</td>
      <td>12</td>
      <td>Female</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>138</td>
    </tr>
    <tr>
      <th>398</th>
      <td>399</td>
      <td>37.728</td>
      <td>2525</td>
      <td>192</td>
      <td>1</td>
      <td>44</td>
      <td>13</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>0</td>
    </tr>
    <tr>
      <th>399</th>
      <td>400</td>
      <td>18.701</td>
      <td>5524</td>
      <td>415</td>
      <td>5</td>
      <td>64</td>
      <td>7</td>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>966</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Obs</th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
  </tbody>
</table>
</div>



The rows of the `DataFrame` have a numerical index (in bold above), which is the default behaviour. As usual in Python, the index starts from zero. This does not need to be the case in the `DataFrame`, but pandas follows the Python convention by default.

Alternatively, we can specify an index variable for the `DataFrame` (that is, a label for each row), which does not need to be a number. For example, if you have time series data, it can be the date.  

We can see that for the `Credit` dataset, the first column is an observation index. Thus, we can directly identify it as the index variable and assign it to the `DataFrame` index when reading the original data file. 


```python
data=pd.read_csv('Data/Credit.csv', index_col='Obs')
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>3</th>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
    <tr>
      <th>4</th>
      <td>148.924</td>
      <td>9504</td>
      <td>681</td>
      <td>3</td>
      <td>36</td>
      <td>11</td>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>964</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55.882</td>
      <td>4897</td>
      <td>357</td>
      <td>2</td>
      <td>68</td>
      <td>16</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>331</td>
    </tr>
  </tbody>
</table>
</div>



Another useful method to use at this stage is `info`, which allows you to check total number of rows and columns, the data types for each column (called `dtypes` in pandas), and verify whether there are missing values. 


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 400 entries, 1 to 400
    Data columns (total 11 columns):
    Income       400 non-null float64
    Limit        400 non-null int64
    Rating       400 non-null int64
    Cards        400 non-null int64
    Age          400 non-null int64
    Education    400 non-null int64
    Gender       400 non-null object
    Student      400 non-null object
    Married      400 non-null object
    Ethnicity    400 non-null object
    Balance      400 non-null int64
    dtypes: float64(1), int64(6), object(4)
    memory usage: 37.5+ KB
    

The `object` dtype is the most general type: a column with this dtype will typically contain text data. If you expect a column to be numerical but it appears as `object`, that tells you that there is an issue that you need to investigate and clean up. 

There are no missing values in this dataset since `info` tells us that the dataframe has 400 entries and all columns have 400 non-null values. A direct way of counting missing values is as follows. 


```python
data.isnull().sum()
```




    Income       0
    Limit        0
    Rating       0
    Cards        0
    Age          0
    Education    0
    Gender       0
    Student      0
    Married      0
    Ethnicity    0
    Balance      0
    dtype: int64



### 4. Data selection

There are two ways to select data in pandas: by providing the column and index labels or passing a numerical index. 

**4.1 Selecting a column by label**

The output will now look different because selecting only one column returns a `Series` (a specialised object for a single data series) rather than a `DataFrame`. 


```python
data['Income'].head()
```




    Obs
    1     14.891
    2    106.025
    3    104.593
    4    148.924
    5     55.882
    Name: Income, dtype: float64



**4.2 Selecting multiple columns by label**


```python
data[['Income','Education']].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>104.593</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>148.924</td>
      <td>11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55.882</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
</div>



Note that we passed desired column labels as a Python list. The example will make this clear. 


```python
names=['Income','Education']
print(names)
```

    ['Income', 'Education']
    


```python
data[names].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>104.593</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>148.924</td>
      <td>11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55.882</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
</div>



**4.3 Selecting a column by numerical index**

The `iloc` method allows us to select data by numerical indexing. We just have to be careful not be confused by zero indexing. If we want the first column, then the index is zero. 


```python
data.iloc[:,0].head()
```




    Obs
    1     14.891
    2    106.025
    3    104.593
    4    148.924
    5     55.882
    Name: Income, dtype: float64



The `:` syntax indicates that we want to include all rows in the selection. 

**4.4 Selecting multiple columns by numerical indexes**

Here, we pass a list of column numbers for indexing. 


```python
data.iloc[:,[0,5]].head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



Another method is slicing. Suppose that we want to select the data from the 1st to the 6th column.  When specifying a range of integer indexes, the last one is not included. For example, the cell below requests indexes 0, 1, 2, 3, 4, 5 (first to the sixth column).


```python
data.iloc[:,0:6].head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



**4.5 Selecting rows by labels**

The `loc` method alows one to select rows by the designated index labels (`Obs`). 


```python
data.loc[[1,2,5],:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55.882</td>
      <td>4897</td>
      <td>357</td>
      <td>2</td>
      <td>68</td>
      <td>16</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>331</td>
    </tr>
  </tbody>
</table>
</div>



Unlike slicing with numerical indexing, slicing with label indexes includes the last item. 


```python
data.loc[1:2,:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
  </tbody>
</table>
</div>



This next example makes this part clearer by demonstrating how a `DataFrame` can have a non-numerical index (row labels). 


```python
df = data.head(5).copy() # make a copy of the first five rows of the dataframe
df.index = ['a','b','c', 'd', 'e'] # replace the index with arbitrary labels
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>b</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>c</th>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
    <tr>
      <th>d</th>
      <td>148.924</td>
      <td>9504</td>
      <td>681</td>
      <td>3</td>
      <td>36</td>
      <td>11</td>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>964</td>
    </tr>
    <tr>
      <th>e</th>
      <td>55.882</td>
      <td>4897</td>
      <td>357</td>
      <td>2</td>
      <td>68</td>
      <td>16</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>331</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[['a','c'],:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>c</th>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc['a':'c',:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>b</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>c</th>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
  </tbody>
</table>
</div>



**4.6 Selecting rows by numerical index**

This is useful when the index variable is a string or date. Here, we are back to zero indexing. 


```python
data.iloc[:2,:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[:2,:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>b</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
  </tbody>
</table>
</div>



**4.7 Jointly selecting rows and columns**

We can combine the previous examples to simultaneously select specific rows and columns. 


```python
data.loc[1:2,['Income', 'Education']] # label indexing
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.iloc[:2,[0,5]]  # numerical indexing
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



**4.8 Combining labels and numerical indexes**

As a more advanced concept, a slice of a `DataFrame` is itself a pandas object. That means that we can chain the syntax to select data using a mix of label and numerical indexing.


```python
data[['Income', 'Education']].iloc[0:2,:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Education</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



**4.9 Selecting columns by data type**

We sometimes want select all columns of a particular `dtype` so that we can process them accordingly. You can do this as follows. 


```python
text_variables=data.select_dtypes(['object'])
text_variables.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
    </tr>
  </tbody>
</table>
</div>



### 5. Conditional data selection

Suppose that we want to know the average credit card balance among female customers. Below, we select the rows of the balance column for female customers only.


```python
data.loc[data['Gender']=='Female','Balance'].mean()
```




    529.536231884058



This is called Boolean indexing in Python, because it involves the creation of binary variables indicating whether the condition is `True` of `False` for each row. The next cell will help you to understand this.


```python
print(data['Gender'].head())
print('')
print((data['Gender']=='Female').head())
```

    Obs
    1      Male
    2    Female
    3      Male
    4    Female
    5      Male
    Name: Gender, dtype: object
    
    Obs
    1    False
    2     True
    3    False
    4     True
    5    False
    Name: Gender, dtype: bool
    

You can also specify multiple conditions. The following cell selects females with age equal or lower than 30.  


```python
data.loc[(data['Gender']=='Female') & (data['Age']<=30),'Balance'].mean()
```




    499.59090909090907



See this <a href="http://www.tutorialspoint.com/python/python_basic_operators.htm" target="_blank">reference page</a> for a list of Python comparison operators. 

### 6. Assigning new values

The <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.unique.html" target="_blank">unique</a> method from pandas allows us to view all the unique values in a column. 


```python
data['Gender'].unique()
```




    array([' Male', 'Female'], dtype=object)



We note that the first character in "Male" above is a space, which is a minor error in the data.  This is a common type of error, and more problematically we can have a mix of correct and incorrect labels. 

We can use our new knowledge of data selection to fix this. Below, we joinly select the rows where the gender is " Male" and the Gender column.  We then replace the values in those locations with the correct label. 


```python
data.loc[data['Gender']==' Male','Gender']='Male'
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Gender</th>
      <th>Student</th>
      <th>Married</th>
      <th>Ethnicity</th>
      <th>Balance</th>
    </tr>
    <tr>
      <th>Obs</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14.891</td>
      <td>3606</td>
      <td>283</td>
      <td>2</td>
      <td>34</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106.025</td>
      <td>6645</td>
      <td>483</td>
      <td>3</td>
      <td>82</td>
      <td>15</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Asian</td>
      <td>903</td>
    </tr>
    <tr>
      <th>3</th>
      <td>104.593</td>
      <td>7075</td>
      <td>514</td>
      <td>4</td>
      <td>71</td>
      <td>11</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>580</td>
    </tr>
    <tr>
      <th>4</th>
      <td>148.924</td>
      <td>9504</td>
      <td>681</td>
      <td>3</td>
      <td>36</td>
      <td>11</td>
      <td>Female</td>
      <td>No</td>
      <td>No</td>
      <td>Asian</td>
      <td>964</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55.882</td>
      <td>4897</td>
      <td>357</td>
      <td>2</td>
      <td>68</td>
      <td>16</td>
      <td>Male</td>
      <td>No</td>
      <td>Yes</td>
      <td>Caucasian</td>
      <td>331</td>
    </tr>
  </tbody>
</table>
</div>




```python
data['Gender'].unique()
```




    array(['Male', 'Female'], dtype=object)



### 7. Descriptive statistics

After loading and preparing the data, we can start exploring it by looking at the basic descriptive statitistics. The `describe` method provides a table with basic summary statistics for the data.  


```python
data.describe().round(1) 
# I appended round to limit the number of decimal places in the output, try without it
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
      <th>Rating</th>
      <th>Cards</th>
      <th>Age</th>
      <th>Education</th>
      <th>Balance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>400.0</td>
      <td>400.0</td>
      <td>400.0</td>
      <td>400.0</td>
      <td>400.0</td>
      <td>400.0</td>
      <td>400.0</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>45.2</td>
      <td>4735.6</td>
      <td>354.9</td>
      <td>3.0</td>
      <td>55.7</td>
      <td>13.4</td>
      <td>520.0</td>
    </tr>
    <tr>
      <th>std</th>
      <td>35.2</td>
      <td>2308.2</td>
      <td>154.7</td>
      <td>1.4</td>
      <td>17.2</td>
      <td>3.1</td>
      <td>459.8</td>
    </tr>
    <tr>
      <th>min</th>
      <td>10.4</td>
      <td>855.0</td>
      <td>93.0</td>
      <td>1.0</td>
      <td>23.0</td>
      <td>5.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>21.0</td>
      <td>3088.0</td>
      <td>247.2</td>
      <td>2.0</td>
      <td>41.8</td>
      <td>11.0</td>
      <td>68.8</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>33.1</td>
      <td>4622.5</td>
      <td>344.0</td>
      <td>3.0</td>
      <td>56.0</td>
      <td>14.0</td>
      <td>459.5</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>57.5</td>
      <td>5872.8</td>
      <td>437.2</td>
      <td>4.0</td>
      <td>70.0</td>
      <td>16.0</td>
      <td>863.0</td>
    </tr>
    <tr>
      <th>max</th>
      <td>186.6</td>
      <td>13913.0</td>
      <td>982.0</td>
      <td>9.0</td>
      <td>98.0</td>
      <td>20.0</td>
      <td>1999.0</td>
    </tr>
  </tbody>
</table>
</div>



There are also individual functions for a range of summary statistics. Refer to the [pandas documentation](http://pandas.pydata.org/pandas-docs/stable/api.html#api-dataframe-stats) for a full list of available functions. As examples, we calculate below: (a) the mean for all variables in the dataset (b) the correlation between income and credit card limit. 


```python
data.mean().round(2)
```




    Income         45.22
    Limit        4735.60
    Rating        354.94
    Cards           2.96
    Age            55.67
    Education      13.45
    Balance       520.02
    dtype: float64




```python
data[['Income','Limit']].corr().round(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Limit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Income</th>
      <td>1.00</td>
      <td>0.79</td>
    </tr>
    <tr>
      <th>Limit</th>
      <td>0.79</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>



### 8. Exporting data

Once you have made the necessary modications to the dataset, you may want to save it to continue working on it later. More generally, you may wish to save the results of your analysis or export tables so that they insert them in a report or webpage (after formatting). Pandas has methods to export data as <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_csv.html" target="_blank">csv</a> and <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_excel.html" target="_blank">Excel</a> files, <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_latex.html" target="_blank">LaTex</a> and <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_html.html" target="_blank">HTML</a> tables, among <a href='http://pandas.pydata.org/pandas-docs/stable/api.html#id12' target="_blank">other options</a>.

In the following example I export our <TT>DataFrame</TT> as an Excel file. You can run the cell and try to open the file in Excel to check that it worked. 


```python
data.to_excel('new_data.xlsx')
```

### 9. Credits

Credits to [Dr Marcel Scharth](https://www.linkedin.com/in/marcel-scharth-aa5bab112/?originalSubdomain=au)
