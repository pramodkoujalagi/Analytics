# 📊 Customer Analytics: Preparing Data for Modeling 🛠️

## Project Overview
I've been tasked with optimizing the memory usage and efficiency of the customer data stored in `customer_train.csv`. The goal is to create a cleaned DataFrame named `ds_jobs_clean`, adhering to specific requirements outlined by the company's data team.

## Requirements 📋
1. Columns with integers must be stored as 32-bit integers (int32).
2. Columns with floats must be stored as 16-bit floats (float16).
3. Columns with nominal categorical data must be stored as the category data type.
4. Columns with ordinal categorical data must be stored as ordered categories, reflecting their natural order.
5. The order of columns in `ds_jobs_clean` must match that of the original dataset.
6. Filter the DataFrame to include only students with 10 or more years of experience at companies with at least 1000 employees.

## Dataset 📊
- **File:** customer_train.csv
- **DataFrame:** ds_jobs

## Code 🖥️
```python
import pandas as pd

# Load the dataset
ds_jobs = pd.read_csv("customer_train.csv")

# Copy the DataFrame for cleaning
ds_jobs_clean = ds_jobs.copy()

# Define ordered categorical data
ordered_cats = {
    'relevant_experience': ['No relevant experience', 'Has relevant experience'],
    'enrolled_university': ['no_enrollment', 'Part time course', 'Full time course'],
    'education_level': ['Primary School', 'High School', 'Graduate', 'Masters', 'Phd'],
    'experience': ['<1'] + list(map(str, range(1, 21))) + ['>20'],
    'company_size': ['<10', '10-49', '50-99', '100-499', '500-999', '1000-4999', '5000-9999', '10000+'],
    'last_new_job': ['never', '1', '2', '3', '4', '>4']
}

# Efficiently change data types
for col in ds_jobs_clean:
    if ds_jobs_clean[col].dtype == 'int':
        ds_jobs_clean[col] = ds_jobs_clean[col].astype('int32')
    elif ds_jobs_clean[col].dtype == 'float':
        ds_jobs_clean[col] = ds_jobs_clean[col].astype('float16')
    elif col in ordered_cats.keys():
        category = pd.CategoricalDtype(ordered_cats[col], ordered=True)
        ds_jobs_clean[col] = ds_jobs_clean[col].astype(category)
    else:
        ds_jobs_clean[col] = ds_jobs_clean[col].astype('category')

# Filter relevant students
ds_jobs_clean = ds_jobs_clean[(ds_jobs_clean['experience'] >= '10') & (ds_jobs_clean['company_size'] >= '1000-4999')]
```

## Memory Optimization 🧠
After preprocessing, calling `.info()` or `.memory_usage()` on `ds_jobs_clean` should reveal a substantial decrease in memory usage, ensuring efficient storage and processing of the dataset.

## Conclusion 🎉
By adhering to the outlined requirements and optimizing memory usage, I've successfully prepared the customer data for modeling, facilitating streamlined analysis and insights generation.
