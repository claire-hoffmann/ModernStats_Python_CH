---
title: Data Transformation
teaching: 40
exercises: 20
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

Words are useful, but what's more useful are the sentences and stories we build with them.
Similarly, while a lot of powerful, general tools are built into Python,
specialized tools built up from these basic units live in
[libraries](../learners/reference.md#library)
that can be called upon when needed.

## Introduction

### What is a Package/Library in Python?

In Python, a [library](<(../learners/reference.md#library)>) (or package) is a collection of pre-written code that you can use to perform common tasks without needing to write the code from scratch. It's a toolbox that provides tools (functions, classes, and modules) to help you solve specific problems.

Python packages save time and effort by providing solutions to common programming challenges. Instead of reinventing the wheel, you can `import` these tools into your scripts and use them to complete tasks more easily.

### A Python package for data manipulations: `pandas`

In this lesson, we will focus on using the **pandas** library in Python to perform common data wrangling tasks.

`pandas` is an open-source Python library for data manipulation and analysis. It provides data structures like **DataFrame** and **Series** that make it easy to handle and analyze data.

#### `Series`

A Series is a one-dimensional labeled array, similar to a list. It can hold any data type, such as integers, strings, or even more complex data types.

Key Features:

- It’s one-dimensional, so it holds data in a single column.
- It has an index (labels) that you can use to access specific elements.
- Each element in the Series has a label (index) and a value.

Example of a Series:

```python
import pandas as pd

# Create a Series from a list

data = [10, 20, 30, 40, 50]
series = pd.Series(data)

# Print the Series

print(series)
```

Output:

```css
0 10
1 20
2 30
3 40
4 50
dtype: int64
```

The index is 0, 1, 2, 3, 4, and the values are 10, 20, 30, 40, 50.
pandas automatically creates an index for you (starting from 0), but you can also specify a custom index.

#### `DataFrame`

A DataFrame is a two-dimensional, table-like structure (similar to a spreadsheet or SQL table) that can hold multiple Series. It is the most commonly used pandas object.

A DataFrame consists of:

- Rows (with an index, just like a Series),
- Columns (which are each Series).

You can think of a DataFrame as a collection of Series that share the same index.

Key Features:

- It’s two-dimensional (i.e., it has rows and columns).
- Each column is a Series.
- It has both row and column labels (indexes and column names).
- It can hold multiple data types (integers, strings, floats, etc.).

Example of a DataFrame:

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

```css
Name Age City
0 Alice 25 New York
1 Bob 30 Los Angeles
2 Charlie 35 Chicago
```

Here:

- The **rows** are indexed from 0, 1, 2 (default index).
- The **columns** are `Name`, `Age`, and `City`.
- Each column is a **Series**, so the `Name` column is a Series, the `Age` column is another Series, etc.

## Loading data

### Loading CSV data

You can load a CSV file into a pandas DataFrame using the `read_csv()` function:

```python
df = pd.read_csv('path/to/file.csv')
print(df.head())
```

- `read_csv()` reads the CSV file located at 'path/to/file.csv' and loads it into a pandas DataFrame (`df`).
- By default, it assumes that the file has a header row (i.e., column names) in the first row.

If the file does not have a header, you can use the `header=None` parameter to let pandas generate default column names.

```python
df = pd.read_csv('path/to/file.csv', header=None)
```

You can pass arguments like `sep` if the file uses a different delimiter (e.g., tab-separated \t).

```python
df = pd.read_csv('path/to/file.csv', sep=`t`)
```

### Loading Excel data

Pandas provides the `read_excel()` function for reading Excel files.

```python
df = pd.read_excel('path/to/file.xlsx')
```

You can specify the sheet name if the file contains multiple sheets. By default, it will load the first sheet.

```python
df = pd.read_excel('data/sales_data.xlsx', sheet_name='Q1_2024')
```

You can also load multiple sheets into a dictionary of DataFrames using `sheet_name=None`.

```python
df = pd.read_excel('data/sales_data.xlsx', sheet_name=None)
```

### Other file formats

You can also load data from other formats like JSON, or SQL databases:

- JSON: `pd.read_json('your_file.json')`
- SQL: `pd.read_sql('SELECT * FROM table', connection)`

## Data exploration

Once you've loaded your data, it's important to understand its structure and contents.

### Viewing the first few rows

```python
print(df.head())  # Shows the first 5 rows
```

If you want to see more (or fewer) rows, you can pass a number to `head()`, such as `df.head(10)` to view the first 10 rows.

Similarly, you can use the `tail()` method to view the last few rows of the DataFrame.

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

### Checking for Missing Values

The `isnull()` method returns a DataFrame of the same shape as `df`, where each element is a boolean (`True` for missing values and `False` for non-missing values).

```python
print(df.isnull())
df.isnull().sum()
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

## Cleaning data

### Renaming columns

You may want to rename columns for clarity:

```python
df.rename(columns={'old_name': 'new_name'}, inplace=True)
```

### Dropping columns

If you no longer need a column, you can drop it:

```python
df.drop(columns=['column_name'], inplace=True)
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

This will remove duplicates based only on column 'A'.

### Handling missing data

Missing data is common in real-world datasets. You can handle it in various ways:

- Dropping rows with missing values:

```python
df.dropna(inplace=True)
```

- Filling missing values with a default value:

```python
df.fillna(0, inplace=True)  # Fill missing values with 0
```

## Transforming data

### Filtering rows

You can filter rows based on certain conditions. For example, to filter for rows where the column age is greater than 30:

```python
df_filtered = df[df['age'] > 30]
```

### Sorting data

To sort your DataFrame by a specific column:

```python
df_sorted = df.sort_values(by='column_name', ascending=False)
```

### Creating new columns

You can create new columns based on existing ones. For example:

```python
df['new_column'] = df['column1'] + df['column2']
```

## Merging and joining data

Often, you need to combine multiple datasets. pandas makes it easy to merge and join DataFrames.

### Merging dataFrames

```python
df_merged = pd.merge(df1, df2, on='common_column')
```

### Concatenating dataFrames

You can concatenate DataFrames vertically (stacking rows) or horizontally (stacking columns):

```python
df_combined = pd.concat([df1, df2], axis=0)  # Vertical stacking
```

## Aggregating data

Aggregation is often used to summarize data by applying functions like sum, mean, etc., to groups of rows.

### Grouping data

The `groupby()` method in pandas is used to group data by one or more columns. Once the data is grouped, you can apply an aggregation function to each group.

```python
df_grouped = df.groupby('column_name').agg({'numeric_column': 'mean'})
```

#### Basic Grouping

Let's assume we have a dataset of sales data that includes the following columns: store, product, and sales.

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

```css
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

```css
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

#### Calculating the Mean

To calculate the average sales per store, you can use the `mean()` function:

```python
mean_sales = df.groupby('store')['sales'].mean()
print(mean_sales)
```

Output:

```css
store
A 33.333333
B 50.000000
C 55.000000
Name: sales, dtype: float64
```

#### Calculating the Count

You can also count how many rows there are in each group. This is useful when you want to know how many entries exist for each group:

```python
count_sales = df.groupby('store')['sales'].count()
print(count_sales)
```

Output:

```css
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

```css
store
A 60
B 50
C 10
Name: sales, dtype: int64
```

In this example, we used a lambda function to compute the range of sales for each store.

### Handling missing values during aggregation

When aggregating data, missing values (NaN) are typically ignored by default. However, if you need to change this behavior, you can control how pandas handles them using the `skipna` argument.

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

```css
store
A     100.0
B     150.0
C      NaN
Name: sales, dtype: float64
```

Notice that the sum for store C is NaN because the `df` dataframe contains a missing value.

### Aggregating while preserving the data structure

The `transform()` function in pandas allows you to perform transformations on a group of data while preserving the original structure. Unlike aggregation (which reduces data), `transform()` returns a `DataFrame` or `Series` with the same index as the original.

If you want to rank the sales data within each store, you can use the `rank()` function inside transform():

```python
# Rank the sales within each store
df['sales_rank'] = df.groupby('store')['sales'].transform('rank')
print(df)
```

Output:

```css
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

You can also specify other options like separator (sep), columns to export, or if you want to handle missing values with the na_rep parameter.

```python
df.to_csv('output_data.tsv', sep='\t', index=False)
```

### Exporting to Excel

```python
df.to_excel('cleaned_data.xlsx', index=False)
```

You can also specify which sheet name to use with the `sheet_name` parameter:

```Python
df.to_excel('output_data.xlsx', sheet_name='Sheet1', index=False)
```

If you're dealing with multiple DataFrames and need to save them in different sheets of the same Excel file, you can use an ExcelWriter:

```Python
with pd.ExcelWriter('output_data.xlsx') as writer:
    df.to_excel(writer, sheet_name='Sheet1', index=False)
    df.to_excel(writer, sheet_name='Sheet2', index=False)
```

### Other supported export formats

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
