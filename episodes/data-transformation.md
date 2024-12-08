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

Data wrangling is the process of cleaning and transforming raw data into a format that is more useful for analysis. In this lesson, we will focus on using the **pandas** library in Python to perform common data wrangling tasks.

By the end of this lesson, you should feel comfortable with the basic tools available in pandas for preparing your data for analysis.

---

## Prerequisites

Before starting this lesson, make sure you have the following prerequisites:

- Basic understanding of Python
- Python installed on your computer
- **pandas** and **numpy** installed (`pip install pandas numpy`)

---

## Introduction to `pandas`

### What is `pandas`?

`pandas` is an open-source Python library for data manipulation and analysis. It provides data structures like **DataFrame** and **Series** that make it easy to handle and analyze data.

### Importing `pandas`

To start using pandas, you need to import it:

```python
import pandas as pd
```

## Loading data

### Loading CSV data

The most common format for datasets is CSV (Comma Separated Values). You can load a CSV file into a pandas DataFrame using the `read_csv()` function:

```python
df = pd.read_csv('your_file.csv')
print(df.head())
```

### Other file formats

You can also load data from other formats like Excel, JSON, or SQL databases:

- Excel: `pd.read_excel('your_file.xlsx')`
- JSON: `pd.read_json('your_file.json')`
- SQL: `pd.read_sql('SELECT * FROM table', connection)`

## Data exploration

Once you've loaded your data, it's important to understand its structure and contents.

### Viewing the first few rows

```python
print(df.head())  # Shows the first 5 rows
```

### Summary statistics

To get a quick overview of the numerical data, you can use:

```python
print(df.describe())
```

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

### Pivot tables

A pivot table is a powerful tool for summarizing data in a tabular format, allowing you to transform long-form data into a more readable, structured summary.

To create a pivot table in pandas, you use the `pivot_table()` function. Let's create a pivot table to summarize the total sales by store and product:

```python
pivot_table = df.pivot_table(values='sales', index='store', columns='product', aggfunc='sum')
print(pivot_table)
```

Output:

```css
product  apple  banana
store
A          10      90
B          30      40
C          50      60
```

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

### Exporting to Excel

```python
df.to_excel('cleaned_data.xlsx', index=False)
```
