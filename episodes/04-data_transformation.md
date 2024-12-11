---
title: Data Transformation
teaching: 60
exercises: 60
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what a library is and what libraries are used for.
- Import a Python library and use the functions it contains.
- Read tabular data from a file into a program.
- Clean and prepare data.
- Merge and reshape data.
- Handle missing values.
- Aggregate and summarize data.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I process tabular data files in Python?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

### What is a Package/Library in Python?

In Python, a [library](<(../learners/reference.md#library)>) (or package) is a collection of pre-written code that you can use to perform common tasks without needing to write the code from scratch. It's a toolbox that provides tools (functions, classes, and modules) to help you solve specific problems.

Python packages save time and effort by providing solutions to common programming challenges. Instead of reinventing the wheel, you can `import` these tools into your scripts and use them to complete tasks more easily.

### A Python package for data manipulations: `pandas`

In this lesson, we will focus on using the **`pandas`** library in Python to perform common data wrangling tasks.

`pandas` is an open-source Python library for data manipulation and analysis. It provides data structures like **`DataFrame`** and **`Series`** that make it easy to handle and analyze data.

#### `Series`

A `Series` is a one-dimensional labeled array, similar to a list. It can hold any data type, such as integers, strings, or even more complex data types.

Key Features:

- It’s one-dimensional, so it holds data in a single column.
- It has an index (labels) that you can use to access specific elements.
- Each element in the Series has a label (index) and a value.

Example of a `Series`:

```python
import pandas as pd

# Create a Series from a list

data = [10, 20, 30, 40, 50]
series = pd.Series(data)

# Print the Series

print(series)
```

Output:

```output
0 10
1 20
2 30
3 40
4 50
dtype: int64
```

The index is 0, 1, 2, 3, 4, and the values are 10, 20, 30, 40, 50.
`pandas` automatically creates an index for you (starting from 0), but you can also specify a custom index.

#### `DataFrame`

A `DataFrame` is a two-dimensional, table-like structure (similar to a spreadsheet or SQL table) that can hold multiple `Series`. It is the most commonly used `pandas` object.

A `DataFrame` consists of:

- Rows (with an index, just like a `Series`),
- Columns (which are each `Series`).

You can think of a `DataFrame` as a collection of `Series` that share the same index.

Key Features:

- It’s two-dimensional.
- Each column is a `Series`.
- It has both row and column labels (indexes and column names).
- It can hold multiple data types (integers, strings, floats, etc.).

Example of a `DataFrame`:

```python
import pandas as pd

# Create a DataFrame using a dictionary

data = {
'Name': ['Alice', 'Bob', 'Charlie'],
'Age': [25, 30, 35],
'City': ['New York', 'Paris', 'Seoul']
}
df = pd.DataFrame(data)

# Print the DataFrame

print(df)
```

Output:

```output
Name Age City
0 Alice 25 New York
1 Bob 30 Los Angeles
2 Charlie 35 Chicago
```

- The **rows** are indexed from 0, 1, 2 (default index).
- The **columns** are `Name`, `Age`, and `City`.
- Each column is a **Series**, so the `Name` column is a `Series`, the `Age` column is another `Series`, etc.

::::::::::::::::::::::::::::::::::::::::: callout

## Import methods

In Python, libraries (or modules) can be imported into your code using the `import` statement. This allows you to access the functions, classes, and methods defined in that library. There are several ways to do it:

1. Full import: `import pandas`

- Use with `pandas.DataFrame()`, `pandas.Series()`, etc.

2. Import with alias: `import pandas as pd`

- Use with `pd.DataFrame()`, `pd.Series()`, etc.

3. Import specific functions or classes: `from pandas import DataFrame` 

- Use directly as `DataFrame()`.

4. Import multiple specific elements: `from pandas import DataFrame, Series`

In general, we use the **option 2** for `pandas`.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Loading data

### Loading CSV data

You can load a CSV file into a `pandas` `DataFrame` using the `read_csv()` function:

```python
df = pd.read_csv('path\to\file.csv')
print(df.head())
```

- `read_csv()` reads the CSV file located at `path\to\file.csv` and loads it into a `pandas` `DataFrame` (`df`).
- By default, it assumes that the file has a header row (i.e., column names) in the first row.

If the file does not have a header, you can use the `header=None` parameter to let pandas generate default column names.

```python
df = pd.read_csv('path\to\file.csv', header=None)
```

You can pass arguments like `sep` if the file uses a different delimiter (e.g., tab-separated `\t`).

```python
df = pd.read_csv('path\to\file.csv', sep=`t`)
```

::::::::::::::::::::::::::::::::::::::::: callout

## Raw string literal

In Python, the `r` prefix before a string is used to create a raw string literal. This tells Python to treat the string exactly as it is, without interpreting backslashes (`\`) as escape characters.

```python
df = pd.read_csv(r'path\to\file.csv')
```

In regular strings, backslashes are used as escape characters. For example, `\n` represents a new line.

::::::::::::::::::::::::::::::::::::::::::::::::::

### Loading Excel data

`pandas` provides the `read_excel()` function for reading Excel files.

```python
df = pd.read_excel('path\to\file.xlsx')
```

You can specify the sheet name if the file contains multiple sheets. By default, it will load the first sheet.

```python
df = pd.read_excel('data\sales_data.xlsx', sheet_name='Q1_2024')
```

You can also load multiple sheets into a dictionary of `DataFrames` using `sheet_name=None`.

```python
df = pd.read_excel('data\sales_data.xlsx', sheet_name=None)
```

### Other file formats

You can also load data from other formats like JSON, or SQL databases:

- JSON: `pd.read_json('your_file.json')`
- SQL: `pd.read_sql('SELECT * FROM table', connection)`

## Data exploration

### Viewing the first few rows

```python
print(df.head())  # Shows the first 5 rows
```

If you want to see more (or fewer) rows, you can pass a number to `head()`, such as `df.head(10)` to view the first 10 rows.

Similarly, you can use the `tail()` method to view the last few rows of the `DataFrame`.

### Viewing the columns

```python
print(df.columns)  # Shows the first 5 rows
```

### Unique values in columns

To get a sense of the distinct values in a column, the `unique()` and `value_counts()` methods are useful.

```python
df['column_name'].unique()
```

The `unique()` method shows all the unique values in a column.

```python
df['column_name'].value_counts()
```

The `value_counts()` method returns the count of unique values in the column, sorted in descending order. This is particularly useful for categorical data.

### Checking for missing values

The `isnull()` method returns a `DataFrame` of the same shape as `df`, where each element is a boolean (`True` for missing values and `False` for non-missing values).

```python
print(df.isnull())
```

To get the total number of missing values in each column, you can chain `sum()` to `isnull()`.

```python
print(df.isnull().sum())
```

This gives you a count of how many missing values are present in each column.

### Summary statistics

To get a quick overview of the numerical data, you can use:

```python
print(df.describe())
```

The `describe()` method provides summary statistics for all numeric columns, including:

- `count`: the number of non-null entries
- `mean`: the average value
- `std`: the standard deviation
- `min`/`max`: the minimum and maximum values
- `25%`, `50%`, `75%`: the percentiles

### Checking the data types

To understand the types of data in each column:

```python
print(df.dtypes)
```

::::::::::::::::::::::::::::::::::::::: challenge

## Exploring a SDMX dataset

Using the `education.csv` dataset in the materials for this episode, write the lines of code to:

- Import `pandas`
- Load the dataset into a `pandas` `DataFrame`
- Print the list of columns in this dataset
- Print the unique values of the `REF_AREA` column

::::::::::::::: solution

## Solution

```python
import pandas as pd

df = pd.read_csv(r"education.csv")

print(df.columns)
print(df["REF_AREA"].unique())
```

```output
Index(['STRUCTURE', 'STRUCTURE_ID', 'STRUCTURE_NAME', 'ACTION', 'REF_AREA',
       'Reference area', 'MEASURE', 'Measure', 'UNIT_MEASURE',
       'Unit of measure', 'INST_TYPE_EDU', 'Type of educational institution',
       'EDUCATION_LEV', 'Education level', 'AGE', 'Age', 'SUBJ_TYPE',
       'Subject', 'OBS_VALUE', 'Observation value', 'OBS_STATUS',
       'Observation status', 'UNIT_MULT', 'Unit multiplier',
       'STATISTICAL_OPERATION', 'Statistical operation', 'REF_PERIOD',
       'Reference period', 'DECIMALS', 'Decimals'],
      dtype='object')
['BFR' 'CHN' 'ESP' 'ISL' 'MEX' 'CHE' 'DEU' 'LTU' 'SAU' 'AUS' 'CAN' 'NLD'
 'CHL' 'FRA' 'KOR' 'GRC' 'LUX' 'ROU' 'FIN' 'IND' 'IRL' 'ISR' 'CZE' 'SVK'
 'TUR' 'USA' 'SVN' 'BGR' 'SWE' 'ZAF' 'UKM' 'NZL' 'OECD' 'BFL' 'IDN' 'NOR'
 'DNK' 'HRV' 'HUN' 'ARG' 'CRI' 'EST' 'COL' 'PER' 'POL' 'PRT' 'UKB' 'ITA'
 'BRA' 'JPN' 'LVA' 'EU25' 'G20' 'AUT']
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Cleaning data

### Renaming columns

You may want to rename columns for clarity:

```python
df.rename(columns={'old_name': 'new_name'}, inplace=True)
```

::::::::::::::::::::::::::::::::::::::::: callout

The `inplace=True` parameter means that we are modifying the original `DataFrame` `df` directly.

By default, `inplace=False`, which means the following line won't rename the `old_name` column:

```python
df.rename(columns={'old_name': 'new_name'})
```

An alternative is to create a new `DataFrame` with the renamed columns and assign it back to `df`:

```python
df = df.rename(columns={'old_name': 'new_name'})
```

The `inplace` parameter is present in many other `pandas` methods.

::::::::::::::::::::::::::::::::::::::::::::::::::

### Dropping columns

If you no longer need a column, you can drop it:

```python
df.drop(columns=['column_name'], inplace=True)
```

You can also select specific columns from a `DataFrame` by passing a list of column names to the `DataFrame` inside double brackets.

```python
df = df[["col1", "col2"]]
```

### Removing duplicates

To remove duplicate rows, use the `drop_duplicates()` method, which removes all duplicate rows by default.

```python
df_cleaned = df.drop_duplicates()
```

You can also specify which columns to check for duplicates by passing a subset to the subset parameter:

```python
df_cleaned = df.drop_duplicates(subset=['A'])
```

This will remove duplicates based only on column `A`.

### Handling missing data

You can handle missing data it in various ways:

- Dropping rows with missing values:

```python
df.dropna(inplace=True)
```

- Filling missing values with a default value:

```python
df.fillna(0, inplace=True)  # Fill missing values with 0
```

::::::::::::::::::::::::::::::::::::::::: callout

Some methods, such as `fillna()` can be applied both on `Series` and `DataFrame` objects.

```python
df['A'] = df['A'].fillna(0) # Fill missing values in column 'A' with 0
```

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## First cleaning steps

Using the `education.csv` dataset in the materials for this episode (continuing on the script of the previous exercise), write the lines of code to:

- Keep only the following columns: `REF_AREA`, `AGE`, `SUBJ_TYPE`, `OBS_VALUE`, `REF_PERIOD`
- Rename them with simpler names: `iso3`, `age`, `subject`, `value`, `year`
- Drop rows with missing data

::::::::::::::: solution

## Solution

```python
# Keeping only the necessary columns
df = df[["REF_AREA", "AGE", "SUBJ_TYPE", "OBS_VALUE", "REF_PERIOD"]]

# Rename them
df.rename(columns={
    "REF_AREA": "iso3",
    "AGE": "age",
     "SUBJ_TYPE": "subject",
     "OBS_VALUE": "value",
     "REF_PERIOD": "year"},
     inplace=True)

# Drop rows with missing data
df.dropna(inplace=True)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Transforming data

### Filtering rows

You can filter rows based on certain conditions. For example, to filter for rows where the column `age` is greater than 30:

```python
df_filtered = df[df['age'] > 30]
```

Another way to do this is to use `loc` (Label Based Indexing):

```python
df_filtered = df.loc[df['age'] > 30]
```

### Replacing values based on condition

`loc` can also be used to replace values in a `DataFrame` based on conditions.
Let's assume we have the following `DataFrame`, and we want to update certain values based on specific conditions.

```python
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'Age': [25, 30, 35, 40],
    'City': ['New York', 'Los Angeles', 'Chicago', 'San Francisco']
}

df = pd.DataFrame(data)
print("Original DataFrame:")
print(df)
```

```output
Original DataFrame:
      Name  Age             City
0    Alice   25         New York
1      Bob   30    Los Angeles
2  Charlie   35          Chicago
3    David   40  San Francisco
```

Suppose we want to replace the city for anyone over the age of 30 with Seattle.

```python
# Replace 'City' with 'Seattle' where 'Age' is greater than 30
df.loc[df['Age'] > 30, 'City'] = 'Seattle'

print("\nUpdated DataFrame:")
print(df)
```

```output
Updated DataFrame:
      Name  Age      City
0    Alice   25  New York
1      Bob   30  Los Angeles
2  Charlie   35   Seattle
3    David   40   Seattle
```

- `df['Age'] > 30`: This is the condition used to filter rows where the `Age` is greater than 30.
- `df.loc[df['Age'] > 30, 'City']`: This selects the `City` column for those rows where the condition is true.
- `= 'Seattle'`: This replaces the value in the `City` column with 'Seattle' for those rows.


### Sorting data

To sort your `DataFrame` by a specific column:

```python
df_sorted = df.sort_values(by='column_name', ascending=False)
```

### Creating new columns

You can create new columns based on existing ones. For example:

```python
df['new_column'] = df['column1'] + df['column2']
```

### Replace values using `map`

The `map()` method in `pandas` allows you to apply a mapping or a function to each element in the `Series`. You can use `map()` with a dictionary to replace values in a `Series` according to the mapping provided.

```python
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'City': ['NY', 'LA', 'CHI', 'SF']
    }

df = pd.DataFrame(data)

# Create a dictionary for mapping

city_map = {
    'NY': 'New York',
    'LA': 'Los Angeles',
    'CHI': 'Chicago',
    'SF': 'San Francisco'
    }

# Apply the map function to replace city abbreviations

df['City'] = df['City'].map(city_map)

print(df)
```

```output
Name City
0 Alice New York
1 Bob Los Angeles
2 Charlie Chicago
3 David San Francisco
```

- `city_map` is a dictionary where the keys are the city abbreviations and the values are the full city names.
- `df['City'].map(city_map)`: This replaces the city abbreviations with the corresponding full city names from the `city_map` dictionary.

::::::::::::::::::::::::::::::::::::::: challenge

## Selection and mapping

We are still using the `education.csv` dataset in the materials for this episode (continuing on the script of the previous exercise).

- Now, we would like to focus on a subset of education subjects, instead of using the full list (17 subjects). Write the lines of code to select only the 8 subjects listed below.

> You can use the `isin()` method in pandas is used to filter rows .

- You may have noticed that the column for subjects labels in the raw data was filled with missing values. For a better readability, we will transform subject codes into labels, using the following mapping:
  - `READ`: "Reading, writing and literature"
  - `MATH`: "Mathematics"
  - `NSCI`: "Natural sciences"
  - `SSCI`: "Social sciences"
  - `SLAN`: "Second language"
  - `OLAN`: "Other languages"
  - `PHED`: "Physical education and health"
  - `ARTS`: "Arts"

::::::::::::::: solution

## Solution

```python
# Selecting only the 8 main subjects and assign to a new dataframe
df_subset = df.loc[df['subject'].isin(["READ", "MATH", "NSCI", "SSCI", "SLAN", "OLAN", "PHED", "ARTS"])]

# Adding labels
df_subset["subject_label"] = df_subset["subject"].map({
    "READ": "Reading, writing and literature",
    "MATH": "Mathematics",
    "NSCI": "Natural sciences",
    "SSCI": "Social sciences",
    "SLAN": "Second language",
    "OLAN": "Other languages",
    "PHED": "Physical education and health",
    "ARTS": "Arts"})
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Pivoting data

Pivoting and melting are two important operations for reshaping data in `pandas`. They are used to transform a `DataFrame` from "long" format to "wide" format, and vice versa.

#### Pivot

The `pivot()` method reshapes the data by turning unique values from one column into new columns. It's useful when you want to convert a "long" format `DataFrame` (where each row represents a single observation) into a "wide" format (where each unique value becomes a column).


```python
data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
    'City': ['New York', 'Los Angeles', 'New York', 'Los Angeles'],
    'Temperature': [30, 75, 32, 77],
}

df = pd.DataFrame(data)

# Pivoting the data
pivot_df = df.pivot(index='Date', columns='City', values='Temperature')
print(pivot_df)
```

```output
City            Los Angeles  New York
Date
2021-01-01            75        30
2021-01-02            77        32
```

- The `Date` column is used as the index.
- The `City` column values are turned into new columns.
- The `Temperature` column is used to populate the new DataFrame.

#### Melt

The `melt()` function is the opposite of `pivot()`. It transforms a DataFrame from wide format to long format.

```python
data = {
    'Date': ['2021-01-01', '2021-01-02'],
    'New York': [30, 32],
    'Los Angeles': [75, 77],
}

df = pd.DataFrame(data)

# Melting the data
melted_df = df.melt(id_vars=['Date'], var_name='City', value_name='Temperature')
print(melted_df)
```

```output
         Date           City  Temperature
0  2021-01-01       New York           30
1  2021-01-02       New York           32
2  2021-01-01  Los Angeles           75
3  2021-01-02  Los Angeles           77
```

- The `Date` column remains fixed (as `id_vars`).
- The `New York` and `Los Angeles` columns are melted into a single `City` column, with corresponding values in the `Temperature` column.

::::::::::::::::::::::::::::::::::::::: challenge

## Pivot method

You are given a dataset containing sales information for different products over a few months.

```python
import pandas as pd

# Create the DataFrame
data = {
    'Product': ['A', 'A', 'A', 'B', 'B', 'B', 'C', 'C', 'C'],
    'Month': ['January', 'February', 'March', 'January', 'February', 'March', 'January', 'February', 'March'],
    'Sales': [100, 150, 200, 80, 120, 160, 130, 170, 220]
}

df = pd.DataFrame(data)
```

Use the `pivot()` method to rearrange this DataFrame so that the months become columns, and each product's sales data for each month appears under its respective column.

::::::::::::::: solution

## Solution

```python
pivot_df = df.pivot(index='Product', columns='Month', values='Sales')
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Merging and joining data

### Merging `DataFrames`

The `merge()` function in `pandas` is used to combine two `DataFrames` based on one or more common columns. It's similar to SQL joins.

The basic syntax for merging two `DataFrames` is:

```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None)
```

- `left`: The first `DataFrame`.
- `right`: The second `DataFrame`.
- `how`: The type of merge to perform. Options include:
  - `left`: Use only keys from the left `DataFrame` (like a left join in SQL).
  - `right`: Use only keys from the right `DataFrame` (like a right join in SQL).
  - `outer`: Use keys from both `DataFrames`, filling in missing values with `NaN` (like a full outer join in SQL).
  - `inner`: Use only the common keys (like an inner join in SQL, default option).
- `on`: The column or index level names to join on. If not specified, it will join on columns with the same name in both `DataFrames`.
- `left_on` and `right_on`: Specify columns from left and right `DataFrames` to merge on if the column names are different.

In the following example, we merge `DataFrames` on multiple columns by passing a list to the `on` parameter.

```python
df1 = pd.DataFrame({
    'Name': ['John', 'Anna', 'Peter'],
    'City': ['NY', 'LA', 'SF'],
    'Age': [22, 25, 28]
})

df2 = pd.DataFrame({
    'Name': ['John', 'Anna', 'Peter'],
    'City': ['NY', 'LA', 'DC'],
    'Salary': [50000, 60000, 70000]
})

# Merge on multiple columns
merged_df = pd.merge(df1, df2, how='inner', on=['Name', 'City'])
print(merged_df)
```

```output
    Name City  Age  Salary
0   John   NY   22   50000
1   Anna   LA   25   60000
```

### Concatenating `DataFrames`

In addition to merging `DataFrames`, `pandas` provides the `concat()` function, which is useful for combining `DataFrames` along a particular axis (either rows or columns). While `merge()` is typically used for combining `DataFrames` based on a shared key or index, `concat()` is more straightforward and is generally used when you want to append or stack `DataFrames` together.

The basic syntax for `concat()` is:

```python
pd.concat([df1, df2], axis=0, ignore_index=False, join='outer')
```

- `[df1, df2]`: A list of `DataFrames` to concatenate.
- `axis`: The axis along which to concatenate:
  - `axis=0`: Concatenate along rows (default behavior). This stacks `DataFrames` on top of each other.
  - `axis=1`: Concatenate along columns, aligning `DataFrames` side-by-side.
- `ignore_index`: If `True`, the index will be reset (i.e., it will generate a new index). If `False`, the original indices of the `DataFrames` are preserved.
- `join`: Determines how to handle indices (or columns when `axis=1`):

  - `outer`: Takes the union of the indices (or columns) from both `DataFrames` (default).
  - `inner`: Takes the intersection of the indices (or columns), excluding any non-overlapping indices (or columns).

 When concatenating along rows (which is the default behavior), the `DataFrames` are stacked on top of each other, and the rows are added to the end of the previous `DataFrame`. This is commonly used to combine datasets with the same structure but with different data.

Here is an example for concatenating `DataFrames` with the same columns:

```python
df1 = pd.DataFrame({
    'ID': [1, 2, 3],
    'Name': ['John', 'Anna', 'Peter']
})

df2 = pd.DataFrame({
    'ID': [4, 5],
    'Name': ['Linda', 'James']
})

# Concatenate along rows (stack vertically)
concatenated_df = pd.concat([df1, df2], axis=0, ignore_index=True)
print(concatenated_df)
```

```output
   ID   Name
0   1   John
1   2   Anna
2   3  Peter
3   4  Linda
4   5  James
```

In this case:

- The two DataFrames `df1` and `df2` are stacked vertically.
- The `ignore_index=True` parameter ensures that the index is reset to a default integer index (0 to 4).
- If you didn't set `ignore_index=True`, the original indices from `df1` and `df2` would be preserved.

::::::::::::::::::::::::::::::::::::::: challenge

## Merging datasets

We are given two datasets: one containing employee details and the other containing their department information. We want to merge these two datasets on the common column `Employee_ID` to create a single `DataFrame` that contains employee details with their department names, while making sure we won't drop any observation.

```python
import pandas as pd

# Create the Employee DataFrame
employee_data = {
    'Employee_ID': [101, 102, 103, 104, 105],
    'Employee_Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'Age': [25, 30, 35, 40, 45]
}

employee_df = pd.DataFrame(employee_data)

# Create the Department DataFrame
department_data = {
    'Employee_ID': [101, 102, 103, 106],
    'Department': ['HR', 'Finance', 'IT', 'Marketing']
}

department_df = pd.DataFrame(department_data)

# Display both DataFrames
print("Employee DataFrame:")
print(employee_df)

print("\nDepartment DataFrame:")
print(department_df)
```

::::::::::::::: solution

## Solution

```python
merged_df = pd.merge(employee_df, department_df, on='Employee_ID', how='outer')
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Aggregating data

Aggregation is often used to summarize data by applying functions like sum, mean, etc., to groups of rows.

### Grouping data

The `groupby()` method in pandas is used to group data by one or more columns. Once the data is grouped, you can apply an aggregation function to each group.

```python
df_grouped = df.groupby('column_name').agg({'numeric_column': 'mean'})
```

#### Basic Grouping

Let's assume we have a dataset of sales data that includes the following columns: `store`, `product`, and `sales`.

```python
import pandas as pd

data = {
    'store': ['A', 'A', 'B', 'B', 'C', 'C', 'A', 'B'],
    'product': ['apple', 'banana', 'apple', 'banana', 'apple', 'banana', 'banana', 'apple'],
    'sales': [10, 20, 30, 40, 50, 60, 70, 80]
}

df = pd.DataFrame(data)

# Group by 'store' and calculate the total sales for each store
grouped = df.groupby('store')['sales'].sum()
print(grouped)
```

Output:

```output
store
A    100
B    150
C    110
Name: sales, dtype: int64
```

In this example, we grouped the data by `store` and calculated the total sales (`sum`) for each store.

#### Grouping by Multiple Columns

You can also group by multiple columns. Let's say we want to calculate the total sales per store and per product:

```python
grouped_multiple = df.groupby(['store', 'product'])['sales'].sum()
print(grouped_multiple)
```

Output:

```output
store product
A apple 10
banana 90
B apple 30
banana 40
C apple 50
banana 60
Name: sales, dtype: int64
```

This shows the total `sales` for each combination of `store` and `product`.

### Aggregation Functions

Once you've grouped the data, you can apply different aggregation functions. The most common ones include `sum()`, `mean()`, `min()`, `max()`, and `count()`. These can be used to summarize the data in various ways.

#### Calculating the mean

To calculate the average sales per store, you can use the `mean()` function:

```python
mean_sales = df.groupby('store')['sales'].mean()
print(mean_sales)
```

Output:

```output
store
A 33.333333
B 50.000000
C 55.000000
Name: sales, dtype: float64
```

#### Calculating the count

You can also count how many rows there are in each group. This is useful when you want to know how many entries exist for each group:

```python
count_sales = df.groupby('store')['sales'].count()
print(count_sales)
```

Output:

```output
store
A 3
B 3
C 3
Name: sales, dtype: int64
```

#### Using custom aggregations

You can also apply custom aggregation functions to your grouped data. For example, let's say you want to compute the range (difference between the maximum and minimum) of sales for each store:

```python
range_sales = df.groupby('store')['sales'].agg(lambda x: x.max() - x.min())
print(range_sales)
```

Output:

```output
store
A 60
B 50
C 10
Name: sales, dtype: int64
```

::::::::::::::::::::::::::::::::::::::: challenge

## Aggregating data using `groupby()`

Let's now go back to our script for transforming the education dataset.

The `df_subset` `DataFrame` provides for each country and age, the share of instruction time spent on each of the 8 selected subjets.

Now, we would like to compute the average share of instruction time of each selected subject and country.

::::::::::::::: solution

## Solution

```python
df_average = df_subset.groupby(["iso3", "subject", "subject_label", "year"])["value"].mean().reset_index()
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Handling missing values during aggregation

When aggregating data, missing values (`NaN`) are typically ignored by default. However, if you need to change this behavior, you can control how `pandas` handles them using the `skipna` argument.

For example, if you want to include missing values in your aggregation, you can do the following:

```python
data = {
    'store': ['A', 'A', 'B', 'B', 'C', 'C', 'A', 'B'],
    'product': ['apple', 'banana', 'apple', 'banana', 'apple', 'banana', 'banana', 'apple'],
    'sales': [10, 20, 30, 40, 50, None, 70, 80]
}

df = pd.DataFrame(data)

# Group by store and calculate the sum, including missing values
agg_sales_with_na = df.groupby('store')['sales'].sum(skipna=False)
print(agg_sales_with_na)
```

Output:

```output
store
A     100.0
B     150.0
C      NaN
Name: sales, dtype: float64
```

Notice that the sum for store C is `NaN` because the `df` `DataFrame` contains a missing value.

### Aggregating while preserving the data structure

The `transform()` function in `pandas` allows you to perform transformations on a group of data while preserving the original structure. Unlike aggregation (which reduces data), `transform()` returns a `DataFrame` or `Series` with the same index as the original.

If you want to rank the sales data within each store, you can use the `rank()` function inside `transform()`:

```python
# Rank the sales within each store
df['sales_rank'] = df.groupby('store')['sales'].transform('rank')
print(df)
```

Output:

```output
  store  product  sales  sales_rank
0     A    apple     10         1.0
1     A   banana     20         2.0
2     B    apple     30         1.0
3     B   banana     40         2.0
4     C    apple     50         1.0
5     C   banana     60         2.0
6     A   banana     70         3.0
7     B    apple     80         3.0
```

## Exporting data

Once you've wrangled your data, you may want to export it to a file.

### Exporting to CSV

```python
df.to_csv('cleaned_data.csv', index=False)
```

> `index=False` prevents the row index from being saved in the file.

You can also specify other options like separator (`sep`):

```python
df.to_csv('output_data.tsv', sep='\t', index=False)
```

### Exporting to Excel

```python
df.to_excel('cleaned_data.xlsx', index=False)
```

You can also specify which sheet name to use with the `sheet_name` parameter:

```python
df.to_excel('output_data.xlsx', sheet_name='Sheet1', index=False)
```

If you're dealing with multiple `DataFrames` and need to save them in different sheets of the same Excel file, you can use `ExcelWriter`:

```python
with pd.ExcelWriter('output_data.xlsx') as writer:
    df.to_excel(writer, sheet_name='Sheet1', index=False)
    df.to_excel(writer, sheet_name='Sheet2', index=False)
```

### Other supported export formats

Other supported export formats include:

| Format      | Method                   | Example Usage                           |
| ----------- | ------------------------ | --------------------------------------- |
| **CSV**     | `DataFrame.to_csv()`     | `df.to_csv('output_data.csv')`          |
| **Excel**   | `DataFrame.to_excel()`   | `df.to_excel('output_data.xlsx')`       |
| **JSON**    | `DataFrame.to_json()`    | `df.to_json('output_data.json')`        |
| **SQL**     | `DataFrame.to_sql()`     | `df.to_sql('my_table', conn)`           |
| **HDF5**    | `DataFrame.to_hdf()`     | `df.to_hdf('output_data.h5', key='df')` |
| **Parquet** | `DataFrame.to_parquet()` | `df.to_parquet('output_data.parquet')`  |
| **Feather** | `DataFrame.to_feather()` | `df.to_feather('output_data.feather')`  |
| **Pickle**  | `DataFrame.to_pickle()`  | `df.to_pickle('output')`                |

Each of these export functions has additional parameters for customizing how the data is saved (e.g., file paths, indexes, column selections). You can refer to the pandas documentation for more advanced options for each method.

