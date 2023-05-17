# Icy-Hot Power

*Authors: Penelope King, Garvey Li*


## Introduction

### The Question and the Dataset

still have to add why dataset and question is important to readers

Is the distribution of outage causes the same in both winter and summer?

This EDA project is based on data collected on [major power outage events](https://www.sciencedirect.com/science/article/pii/S2352340918307182#t0005) that occurred in the United States. Each row of data contains information relevant to our question such such as outage times (month, start date), location (U.S. climate regions, climate of location), and what caused the outage (categories such as severe weather, intentional attack, and details about the categories)

## Cleaning and EDA

### Data Cleaning

The first step we took in data cleaning was seeing which columns contained any missing values. We found that some of the columns contained a large number of null values, such as `HURRICANE.NAME`, `CAUSE.CATEGORY.DETAIL`, `DEMAND.LOWW.MW`, and `CUSTOMERS.AFFECTED`. However, since our question didn't require any data from these columns, we decided to leave them as is. What did matter was that the `MONTH` category was missing for 9 rows. These rows were also missing data for `OUTAGE.START.DATE`, and since the rows were not ordered by date, it would not make sense to try to impute any values for either of these categories. Since there were only 9 rows that we couldn't use out of 1534 total rows, we decided that dropping those rows wouldn't significantly affect the distribution of the data or our hypothesis test.

`occ.head()`
<iframe src="resources/occ-head.html" width=800 height=600 frameBorder=0></iframe>

### Univariate Analysis

<iframe src="resources/cause-dist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="resources/season-dist.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

<iframe src="resources/bar-cause-cat.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency: MAR vs MCAR Imputation Tests

<iframe src="resources/missingness-cdfs.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Test

### Hypotheses

### Testing

### Results

### Discussion







