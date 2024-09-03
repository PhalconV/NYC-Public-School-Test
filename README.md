# NYC-Public-School-Test


## Project Overview
Every year, American high school students take SATs, which are standardized tests intended to measure literacy, numeracy, and writing skills. There are three sections - reading, math, and writing, each with a maximum score of 800 points. These tests are extremely important for students and colleges, as they play a pivotal role in the admissions process.
Analyzing the performance of schools is important for a variety of stakeholders, including policy and education professionals, researchers, government, and even parents considering which school their children should attend.
You have been provided with a dataset called schools.csv, which is previewed below.
You have been tasked with answering three key questions about New York City (NYC) public school SAT performance.

## Data Source
DataCamp Data Manipulation in Pandas Course

## Tools
- Python
- DataCamp's DataLab

## Data Cleaning / Preparation
The data was preloaded into pandas for easy access.

```python
# import important librarys
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Read in the data
schools = pd.read_csv("schools.csv")

# Preview the data
schools.head()

# Start coding here...
# Add as many cells as you like...
```

## Exploratory Data Analysis
EDA involved exloring the whole data to answer some key questions, such as:
### Finding information about the dataset
``` python
schools.info()

# number of rows & columns
schools.shape

# missing values count
schools.isna().sum()

# missing values column view
schools.isna().any()

# missing values
schools.isna()

# duplicates
duplicate = schools.duplicated()
duplicate

# describe finds the count, mean, standard deviation, min, percentile, and max
schools.describe()
```

## Data Analysis

### Which NYC schools have the best math results?
```python
# Which NYC schools have the best math results?
# add the scores together
schools["total_SAT"] = schools["average_math"] + schools["average_reading"] + schools["average_writing"]
# select only school_name and total_SAT column
top_10_schools = schools[["school_name", "total_SAT"]].sort_values(by = "total_SAT", ascending = False).head(10)
```

### What are the top 10 performing schools based on the combined SAT scores?
```python
# What are the top 10 performing schools based on the combined SAT scores?
# find 80% of 800
percent_of_800 = 800 * 0.80

# select schools that are 80% upward, sort in descending order
best_math_schools = schools[schools["average_math"] >= percent_of_800 ][["school_name", "average_math"]].sort_values(by = "average_math", ascending = False)
best_math_schools
```

### Chart of Top 10 Schoools by average Mathematics Score
```python
import matplotlib.pyplot as plt

# find 80% of 800
percent_of_800 = 800 * 0.80

# select schools that are 80% upward, sort in descending order
best_math_schools = schools[schools["average_math"] >= percent_of_800 ][["school_name", "average_math"]].sort_values(by = "average_math", ascending = False)
best_math_schools["short_name"] = best_math_schools["school_name"].apply(lambda x: x[:10] + '...' if len(x) > 10 else x)

# Plotting the data
best_math_schools.plot(x = "short_name", y = "average_math", kind = "bar", legend = False)
plt.title("Top 10 Schools by Math Score")
plt.xlabel("School Name")
plt.ylabel("Average Math Score")
plt.xticks(rotation = 45)
plt.show()
```



### Top Overall 7 schools 
```python
# top schools by combine scores
top_schools_by_combine = schools[schools["total_SAT"] >= 1900][["school_name", "total_SAT"]].sort_values(by = "total_SAT", ascending = False)
top_schools_by_combine
```
### Number of Schools in Each Borough
```python
schools_borough = schools.groupby("borough").agg(num_schools=("total_SAT", "size"), average_SAT=("total_SAT", "mean"), std_SAT=("total_SAT", "std"))
schools_borough
```
### Chart of Average SAT Score by Borough
```python
import matplotlib.pyplot as plt

schools_borough = schools.groupby("borough").agg(num_schools=("total_SAT", "size"), average_SAT=("total_SAT", "mean"), std_SAT=("total_SAT", "std"))
schools_borough = schools_borough.sort_values(by="average_SAT", ascending=False)
schools_borough.plot(y="average_SAT", kind="bar", legend = False)
plt.title("Average SAT Score by Borough")
plt.ylabel("Average SAT Score")
plt.xlabel("Borough Name")
plt.xticks(rotation = 45)
plt.show()
```

### Which single borough has the largest standard deviation in the combined SAT score?

```python
# Which single borough has the largest standard deviation in the combined SAT score?
schools["total_SAT"] = schools["average_math"] + schools["average_reading"] + schools["average_writing"]
schools

grouped = schools.groupby("borough").agg(num_schools=("total_SAT", "size"), average_SAT=("total_SAT", "mean"), std_SAT=("total_SAT", "std"))
grouped

largest_std_dev_borough = grouped.loc[grouped["std_SAT"].idxmax()]

largest_std_dev = pd.DataFrame({
    "borough": [largest_std_dev_borough.name],
    "num_schools": [largest_std_dev_borough["num_schools"]],
    "average_SAT": [round(largest_std_dev_borough["average_SAT"], 2)],
    "std_SAT": [round(largest_std_dev_borough["std_SAT"], 2)]
})

largest_std_dev
```

### Findings
- The Borough with the most Schools is Brooklyn
- The Borough with the most Standard Deviation is Mangattan with 89 number of Schools, average_SAT score of 1340.13, and std_SAT 230.29
- The Borough with the list number of Schools is Staten Island
- The school with the most average math score is Stuyvesant High School with score of 754
- The school with the highest total sat score is Stuyvesant High School with score of 2144
