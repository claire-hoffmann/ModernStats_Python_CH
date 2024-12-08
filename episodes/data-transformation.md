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

```python
df_grouped = df.groupby('column_name').agg({'numeric_column': 'mean'})
```

### Pivot tables

Pivot tables allow you to summarize data in a tabular format:

```python
df_pivot = df.pivot_table(values='value_column', index='row_column', columns='column_column')
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
