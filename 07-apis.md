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
APIs typically provide a URL endpoint to access data. For example, the JSONPlaceholder API provides a free-to-use endpoint like:

```
https://jsonplaceholder.typicode.com/posts
```

#### 3. Make an API Request
Use the `requests` library to fetch data from the API. Example:

```python
url = "https://jsonplaceholder.typicode.com/posts"
response = requests.get(url)
data = response.json()
```

- `response.json()` converts the API response into a Python dictionary or list, depending on the structure.

#### 4. Extract Relevant Data
Examine the data structure to find what you need. For example, if the response looks like this:

```json
[
    {
        "userId": 1,
        "id": 1,
        "title": "Sample Title",
        "body": "Sample body text."
    },
    {
        "userId": 1,
        "id": 2,
        "title": "Another Title",
        "body": "More sample text."
    }
]
```
You can extract specific values like:

```python
data_list = [{
    "UserID": item["userId"],
    "ID": item["id"],
    "Title": item["title"],
    "Body": item["body"]
} for item in data]
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
df.to_csv("posts_data.csv", index=False)
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
url = "https://jsonplaceholder.typicode.com/posts"
response = requests.get(url)
data = response.json()

# Extract Data
data_list = [{
    "UserID": item["userId"],
    "ID": item["id"],
    "Title": item["title"],
    "Body": item["body"]
} for item in data]

# Create DataFrame
df = pd.DataFrame(data_list)

# Save DataFrame
df.to_csv("posts_data.csv", index=False)
print("Data saved to posts_data.csv")
```

---

### Key Takeaways
- Use `requests` to fetch API data.
- Convert JSON responses to a Pandas DataFrame.
- Save or analyze the data using Pandas.

Practice with other APIs to build confidence!

- Let's try with the OECD Data Explorer API

