---
title: Fetching data from APIs
teaching: 40
exercises: 1
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn how to fetch data from an API using Python and load it into a Pandas DataFrame for analysis.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How to get data using a public API ?
- How to transform a JSON output to a panda dataframe ?
- Why the API documentation is essential to succeed in getting the expected data ?

::::::::::::::::::::::::::::::::::::::::::::::::::

### Prerequisites
- Basic understanding of Python (variables, functions, and loops).
- Python installed on your computer.
- Install the following libraries: `requests` and `pandas`:

```bash
pip install requests pandas
```

---

### Steps

#### 1. Import Required Libraries
To get started, import the necessary libraries:

```python
import requests
import pandas as pd
```

#### 2. Understand the API Endpoint
For this exercise, we will use the World Development Indicators (WDI) API from the World Bank. This API provides economic and development data for countries. An example endpoint is:

```
http://api.worldbank.org/v2/country/all/indicator/NY.GDP.MKTP.CD?format=json&date=2020:2021
```

This URL fetches GDP data (indicator `NY.GDP.MKTP.CD`) for all countries for the years 2020 and 2021.

#### 3. Make an API Request
Use the `requests` library to fetch data from the API. Example:

```python
url = "http://api.worldbank.org/v2/country/all/indicator/NY.GDP.MKTP.CD?format=json&date=2020:2021"
response = requests.get(url)
data = response.json()
```

- `response.json()` converts the API response into a Python dictionary or list.

#### 4. Extract Relevant Data
Examine the JSON response structure. For example, the WDI API returns a list where the second element contains the actual data. Extract the relevant data for analysis:

```python
# Extract data from JSON response
data_records = data[1]

# Prepare a list for DataFrame creation
data_list = []
for record in data_records:
    country = record["country"]["value"]
    year = record["date"]
    gdp_value = record["value"]
    data_list.append({
        "Country": country,
        "Year": int(year),
        "GDP": gdp_value
    })
```

#### 5. Convert to DataFrame
Use the extracted data to create a Pandas DataFrame:

```python
df = pd.DataFrame(data_list)
print(df.head())
```

#### 6. Save or Analyze the Data
Save the DataFrame as a CSV file for later use:

```python
df.to_csv("wdi_gdp_data.csv", index=False)
```

Or analyze the data directly using Pandas functions like:

```python
print(df.describe())
```

---

### Complete Example Code

```python
import requests
import pandas as pd

# API Request
url = "http://api.worldbank.org/v2/country/all/indicator/NY.GDP.MKTP.CD?format=json&date=2020:2021"
response = requests.get(url)
data = response.json()

# Extract Data
data_records = data[1]
data_list = []
for record in data_records:
    country = record["country"]["value"]
    year = record["date"]
    gdp_value = record["value"]
    data_list.append({
        "Country": country,
        "Year": int(year),
        "GDP": gdp_value
    })

# Create DataFrame
df = pd.DataFrame(data_list)

# Save DataFrame
df.to_csv("wdi_gdp_data.csv", index=False)
print("Data saved to wdi_gdp_data.csv")
```

---

### Key Takeaways
- Use `requests` to fetch API data.
- Convert JSON responses to a Pandas DataFrame.
- Save or analyze the data using Pandas.

Practice with the OECD Data Explorer to build confidence!



Practice with other APIs to build confidence!

- Let's try with the OECD Data Explorer API

