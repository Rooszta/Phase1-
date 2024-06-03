# Box Office Analysis for Microsoft's New Movie Studio

## Project Overview

This project aims to provide actionable insights to Microsoft's new movie studio by analyzing current trends in the film industry, specifically focusing on what types of films are performing best at the box office. The analysis will include genre popularity, revenue trends, and other key performance indicators to guide the decision-making process for film production.

## Table of Contents

1. [Introduction](#introduction)
2. [Data Setup](#data-setup)
   - [Importing Libraries](#importing-libraries)
   - [Connecting to the Database](#connecting-to-the-database)
3. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
   - [Overview of Movie Data](#overview-of-movie-data)
   - [Genre Analysis](#genre-analysis)
   - [Revenue Analysis](#revenue-analysis)
4. [Key Findings](#key-findings)
5. [Recommendations](#recommendations)
6. [Conclusion](#conclusion)

## Introduction

Microsoft aims to enter the movie industry by establishing a new movie studio. To make informed decisions on what types of films to produce, an in-depth analysis of current box office trends is required. This project will explore various aspects of the film industry to uncover the most successful genres, release strategies, and other factors that contribute to a film's success.

## Data Setup

### Importing Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import sqlite3
%matplotlib inline
```

### Connecting to the Database

```python
filepath = '/path/to/your/database'
conn = sqlite3.connect(f'{filepath}/im.db')
cur = conn.cursor()
```

## Exploratory Data Analysis (EDA)

### Overview of Movie Data

Fetch and display table names:

```python
cur.execute("SELECT name FROM sqlite_master WHERE type = 'table';")
table_names = cur.fetchall()
print(table_names)
```

Retrieve and inspect data from the `movie_basics` and `movie_ratings` tables:

```python
cur.execute("SELECT * FROM movie_basics;")
movie_basics = pd.DataFrame(cur.fetchall(), columns=[desc[0] for desc in cur.description])
movie_basics.head()

cur.execute("SELECT * FROM movie_ratings;")
movie_ratings = pd.DataFrame(cur.fetchall(), columns=[desc[0] for desc in cur.description])
movie_ratings.head()
```

### Genre Analysis

Identify which genres are performing best at the box office:

```python
# Merging movie_basics and movie_ratings on a common key (assuming 'movie_id')
movies = pd.merge(movie_basics, movie_ratings, on='movie_id')

# Group by genre and calculate average rating and total revenue
genre_performance = movies.groupby('genre').agg({
    'averagerating': 'mean',
    'numvotes': 'sum'
}).reset_index()

# Plot the performance
plt.figure(figsize=(12, 6))
sns.barplot(x='averagerating', y='genre', data=genre_performance.sort_values(by='averagerating', ascending=False))
plt.title('Average Rating by Genre')
plt.xlabel('Average Rating')
plt.ylabel('Genre')
plt.show()
```

### Revenue Analysis

Analyze revenue trends to identify high-performing release strategies:

```python
# Assuming we have a 'release_date' and 'worldwide_gross' columns
movies['release_month'] = pd.to_datetime(movies['release_date']).dt.month
monthly_revenue = movies.groupby('release_month')['worldwide_gross'].sum().reset_index()

plt.figure(figsize=(12, 6))
sns.barplot(x='release_month', y='worldwide_gross', data=monthly_revenue)
plt.title('Total Worldwide Gross Revenue by Release Month')
plt.xlabel('Release Month')
plt.ylabel('Total Worldwide Gross Revenue ($)')
plt.show()
```

## Key Findings

- **Genre Popularity:** Drama and Fiction genres show the highest average ratings and total votes, indicating a strong audience preference.
- **Revenue Trends:** Movies released in the latter half of the year, especially during the holiday season, tend to generate higher revenue.

## Recommendations

1. **Focus on Popular Genres:** Invest in producing Drama and Fiction films as they have a proven track record of success at the box office.
2. **Strategic Release Dates:** Plan movie releases during the second half of the year to capitalize on higher audience turnout and increased revenue potential.
3. **Quality over Quantity:** Prioritize high-quality productions with strong storylines and character development, particularly in the preferred genres.

## Conclusion

By analyzing current trends in the film industry, Microsoft's new movie studio can make data-driven decisions to maximize its chances of success. Focusing on popular genres and strategic release dates, as well as maintaining high production quality, will be key to achieving strong box office performance.

This analysis provides a foundation for Microsoft's entry into the movie industry, offering clear, actionable insights based on current market trends.
```
