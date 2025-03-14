# DataFrame to Superset

[![PyPI version](https://badge.fury.io/py/dataframe-to-superset.svg)](https://pypi.org/project/dataframe-to-superset/)
[![GitHub](https://img.shields.io/github/issues/lodu/dataframe-to-superset.svg?style=social&label=Issues)](https://github.com/lodu/dataframe-to-superset)

A Python package to upload pandas DataFrames to Superset.
The goal is to visualize your DataFrame inside Superset by providing an easy way to access your data in Superset.

You need a database/datasource in Superset which allows you to upload CSV files. It is recommended to create a separate database/datasource for this purpose and to always keep the name the same when uploading because it overwrites by default. This will help combat clutter created by this package.

## Installation

You can install the package using pip:

```sh
pip install dataframe-to-superset
```

## Usage

There are three ways to upload a pandas DataFrame to Superset:

### 1. Creating an object of `DataFrameToSuperset`

```python
from dataframe_to_superset import DataFrameToSuperset
import pandas as pd

# Create a DataFrameToSuperset object
uploader = DataFrameToSuperset(
    base_url="http://your-superset-instance",
    username="your-username",
    password="your-password",
    provider="db",  # or "ldap"
    database_name="your-database-name",
    schema="public"  # optional, defaults to "public"
)

# Create a pandas DataFrame
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "join_date": pd.to_datetime(["2021-01-01", "2021-06-15", "2021-09-30"])
})

# Upload the DataFrame
accessable_url = uploader.to_superset(df, name="employees_dataset")
print(accessable_url)
```

### 2. Using `upload_dataframe_to_superset`

```python
from dataframe_to_superset import upload_dataframe_to_superset
import pandas as pd

# Create a pandas DataFrame
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "join_date": pd.to_datetime(["2021-01-01", "2021-06-15", "2021-09-30"])
})

# Upload the DataFrame
accessable_url = upload_dataframe_to_superset(
    dataframe=df,
    base_url="http://your-superset-instance",
    username="your-username",
    password="your-password",
    provider="db",  # or "ldap"
    database_name="your-database-name",
    schema="public",  # optional, defaults to "public"
    name="employees_dataset"  # optional, defaults to a generated name
)
print(accessable_url)
```

### 3. Applying the monkey patch

```python
from dataframe_to_superset import monkey_patch_to_allow_df_to_superset
import pandas as pd

# Apply the monkey patch
monkey_patch_to_allow_df_to_superset(
    base_url="http://your-superset-instance",
    username="your-username",
    password="your-password",
    provider="db",  # or "ldap"
    database_name="your-database-name",
    schema="public",  # optional, defaults to "public"
)

# Create a pandas DataFrame
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "join_date": pd.to_datetime(["2021-01-01", "2021-06-15", "2021-09-30"])
})

df.name = 'people'

# Upload the DataFrame using the new method
accessable_url = df.to_superset()
print(accessable_url)
```
