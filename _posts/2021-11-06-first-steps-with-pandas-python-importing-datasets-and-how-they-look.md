---
layout: post
title: "First steps with pandas (Python): importing datasets and how they look"
date: 2021-11-06
permalink: "/first-steps-with-pandas-python-importing-datasets-and-how-they-look/"
categories:
  - "Blog"
tags:
  - "python"
  - "pandas"
  - "import data"
  - "descriptives"
  - "data science"
---


Recently I started to learn Python and, especially, how to manage datasets with it. `pandas` is one of the well-known libraries for data management in Python. So, I would like to share my notes and my first steps with `pandas`.

## 0) Importing `pandas`

First, I import the library with the name `pd`:

```python
import pandas as pd
```

One of the first things I learned in Python is that libraries can be labeled/named as one wants. This is useful to be more efficient when writng code. Since I have labeled `pandas` as `pd`, now it is enough to write `pd` to call the library, and it is no longer necessary to write `pandas`. Labeling the libraries is not mandatory at all, and I could have basically imported the library as `import pandas`. In this case, I should have called pandas as `pandas`.

## 1) Importing data files: csv and excel

I mainly focus on csv and excel files in this post. I concretely import an example excel file from this github website. For that, I create an object called `dt`, which is a data frame. Then, I indicate to Python that I want to use `pandas` library to import the file and the corresponding command: `read_excel()`.

```python
dt = pd.read_excel('https://file-examples-com.github.io/uploads/2017/02/file_example_XLS_50.xls')
```

In case of importing a csv file, the command is `read_csv`. For example, I import a csv file from the New Zeleand Government website:

```python
dt2 = pd.read_csv('https://www.stats.govt.nz/assets/Uploads/Annual-enterprise-survey/Annual-enterprise-survey-2019-financial-year-provisional/Download-data/annual-enterprise-survey-2019-financial-year-provisional-csv.csv')
```

However, to import other data files formats such as SAS, SPSS, STATA, JSON, or SQL, among others, the function would be:

- `.read_sas()`
- `.read_spss()`
- `.read_stata()`
- `.read_sql()`
- `.read_json()`
- `.read_table()`

### Removing objects created: `del()`

Hereafter, I only want to use the `dt` dataset. For that reason, I delete the `dt2` database with the command `del()`:

```python
del(dt2)
```

## 2) How does the dataset look?

Now that the dataset is loaded in the environtment, I want to check how it looks.

### Retrives the first or last observations: `.head()` and `.tail()`

The command `.head()` shows the first 5 observations (rows) of the data frame and all the variables (columns). This command is similar to `head()` in `R`. Remember that before the command, `.head()`, we must indicate the data frame we want to look at:

```python
dt.head()
```

Moreover, the number of observations to be visualized can be indicated. For example, the first 8 observations:

```python
dt.head(8)
```

On the other hand, the command `.tail()` shows the last 5 observations or, similarly, `.tail(n)` the last 'n' observations.

```python
dt.tail()
```

```python
dt.tail(9)
```

However, one might be interested in looking the observations of a specific variable such as:

```python
dt["Gender"].head()
```

```python
dt["Country"].head()
```

### Main information of the dataset: `.info()`

In order to retrieve some information about the data frame and its structure:

```python
dt.info()
```

### Number of observations `len()`:

```python
len(dt)
```

### The main descriptives: `.describe()`

Now, I'm interested in the main descriptive statistics of the numerical variables from the dataset:

```python
dt.describe()
```

Note that in case of not specifying any variable the code will show all the numerical variables from the data frame. However, if I want to check a specific variable:

```python
dt["Age"].describe()
```

### Tabulate a variable `.value_counts()`:

This function is the same than `tabulate` (for Stata users) and `count()` or `table()`(for R users).

```python
dt['Age'].value_counts()
```

```python
dt['Age'].nunique()
```

### Check for missing values `.isna()`:

To check the number of missing values for each variable better to add `.sum()` at the end:

```python
dt.isna().sum()
```

Without the `.sum()` function, a false/true matrix is shown, indicating whether every single value of the data frame is missing or not.

### Sort observations by values of a variable `.sort_values()`:

This function is similar than `sort` (for Stata users) and `arrange()` (for R users). It orders observations from low to high values (ascending):

```python
dt['Age'].sort_values()
dt.sort_values('Age')
```

For descending order:

```python
dt['Age'].sort_values(ascending=False)
dt.sort_values('Age', ascending=False)
```

------------------------------------------------------------------------

In the **next post** on `pandas` I will show how to perform the **first steps on data wrangling**: rename variable, create new ones, filter observations, merge datasets...
