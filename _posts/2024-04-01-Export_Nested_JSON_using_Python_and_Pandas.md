---
title: Export Nested JSON using Python and Pandas
date: 2024-4-1 21:05:00 +0500
categories: [ Python]
tags: [python, pandas, JSON]
---
Python Pandas can be used to export nested JSON structures. You can create a nested JSON structure from a DataFrame and then export it to a JSON file. Here's how you can do it:

1. Import the Pandas library:

```python
import pandas as pd
```

2. Create a DataFrame with nested data. You can use dictionaries and lists within the DataFrame to represent the nested structure:

```python
data = {
    'name': ['John', 'Alice'],
    'info': [
        {'age': 30, 'city': 'New York'},
        {'age': 25, 'city': 'Los Angeles'}
    ]
}

df = pd.DataFrame(data)
```

This creates a DataFrame with a 'name' column and an 'info' column where each entry is a nested dictionary.

3. Export the DataFrame to a JSON file using the `to_json` method with the 'orient' parameter set to 'records' or 'split':

```python
df.to_json('nested_data.json', orient='records')
```

OR

```python
df.to_json('nested_data.json', orient='split')
```

- 'records' will produce JSON with a list of records, where each record is a nested dictionary.
- 'split' will produce JSON with separate 'columns' and 'data' entries, preserving the nested structure.

4. You can now find the exported nested JSON in the 'nested_data.json' file.

This way, you can export nested JSON structures from a Pandas DataFrame. Adjust the DataFrame structure and export options according to your specific data and requirements.