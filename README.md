# Social Media Data Analysis

## Introduction

Social media has become a ubiquitous part of modern life, with platforms such as Instagram, Twitter, and Facebook serving as essential communication channels. Social media data sets are vast and complex, making analysis a challenging task for businesses and researchers alike. In this project, I explore a simulated social media, for example Tweets, data set to understand trends in likes across different categories.

## Project Scope

The objective of this project is to analyze tweets (or other social media data) and gain insights into user engagement. I will explore the data set using visualization techniques to understand the distribution of likes across different categories. Finally, I will analyze the data to draw conclusions about the most popular categories and the overall engagement on the platform.

## Importing Required Libraries

```python
import pandas as pd
from pandasql import sqldf
import numpy as np
import matplotlib.pyplot as plt
import random
```

## Creating Categories List, Data Size, and Random Data

```python
categories = ['Food', 'Travel', 'Fashion',
              'Fitness', 'Music', 'Culture',
              'Family', 'Health', 'Sports']
n = 1000
data_dict = {'Date': pd.date_range('2023-01-01', periods=n),
             'Category': [random.choice(categories) for _ in range(n)],
             'Likes': np.random.randint(0, 10000, size=n)}
```

## Transporting from Dictionary to DataFrame

```python
df = pd.DataFrame(data_dict)
df.tail()
```

## Exploratory Data Analysis (EDA)

### Checking Data Types

```python
df.dtypes
df.info()
```

### Dropping Duplicate Rows

```python
df.duplicated().sum()
```
- Great! Fortunately, there are no duplicate rows.

```python
df.drop_duplicates()
```

### Dropping Missing & Null Values

```python
df.isnull().sum()
```
- Again, no missing or null values.

```python
df = df.dropna()
df.count()
```

## Converting Fields

Even though the `Date` and `Likes` columns are in the correct format, I applied these formatting steps.

```python
df['Date'] = pd.to_datetime(df['Date'])
df['Likes'] = df['Likes'].astype('int64')
```

## Visualizing and Analyzing the Data

### Likes by Category

```python
likes_by_category = df.groupby('Category')['Likes'].sum()
likes_by_category.plot(kind="bar", figsize=(10, 5))
plt.title("Number of Likes by Category")
plt.ylabel("Number of Likes")
plt.xlabel("Category")
plt.xticks(rotation=45)
plt.show()
```

### Pie Chart of Likes Distribution

```python
likes_by_category = df.groupby('Category')['Likes'].sum()
plt.pie(likes_by_category, labels=likes_by_category.index)
plt.title("Distribution of Likes by Category")
plt.show()
```

### Mean of Likes

```python
mean_of_likes = df['Likes'].mean()
print("Mean of the 'Likes' category: {}".format(mean_of_likes))
```

### Number of Tweets by Category

```python
number_of_tweets_by_category = df.groupby('Category')['Category'].count()
number_of_tweets_by_category.plot(kind="bar", figsize=(10, 5), color='green')
plt.title("Number of Tweets by Category")
plt.ylabel("Number of Tweets")
plt.xlabel("Category")
plt.xticks(rotation=45)
plt.show()
```

### Pie Chart of Tweets Distribution

```python
number_of_tweets_by_category = df.groupby('Category')['Category'].count()
plt.pie(number_of_tweets_by_category, labels=number_of_tweets_by_category.index)
plt.title("Distribution of Tweets by Category")
plt.show()
```

### Most Popular Category

```python
query = """
SELECT
    Category,
    COUNT(*) AS Frequency
FROM
    df
GROUP BY
    Category
ORDER BY
    Frequency DESC
"""

result = sqldf(query)
print(result)
```

- The result shows that `Sports` is in first place.
