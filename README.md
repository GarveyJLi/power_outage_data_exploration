# Icy-Hot Power

*Authors: Penelope King, Garvey Li*


## Introduction

### The Question and the Dataset

still have to add why dataset and question is important to readers

Is the distribution of outages across climate regions the same in both winter and summer?

This EDA project is based on data collected on [major power outage events](https://www.sciencedirect.com/science/article/pii/S2352340918307182#t0005) that occurred in the United States. Each row of data contains information relevant to our question such such as outage times (month, start date), location (U.S. climate regions, climate of location), and what caused the outage (categories such as severe weather, intentional attack, and details about the categories)

## Cleaning and EDA

### Data Cleaning

The first step we took in data cleaning was seeing which columns contained any missing values. We found that some of the columns contained a large number of null values, such as `HURRICANE.NAME`, `CAUSE.CATEGORY.DETAIL`, `DEMAND.LOWW.MW`, and `CUSTOMERS.AFFECTED`. However, since our question didn't require any data from these columns, we decided to leave them as null values. 

In order to group the outages by seasons, we had to create a new column, `SEASONS`, based off the column `MONTH`, which contained values `winter`(Dec-Feb), `spring`(Mar-May), `summer`(Jun-Aug), and `fall`(Sept-Nov). As it turns out the `MONTH` and `OUTAGE.START.DATE` categories were missing for 9 of the rows, which meant we had to find another way to determine the season for these outages. 

For one of the rows, we noticed that one of the columns, `CAUSE.CATEGORY.DETAIL`, contained a value `winter storm`, which allowed us to infer that that outage occurred in the winter. 

For the remaining 8 rows that didn't have any explicit information regarding time of year, we decided to use probabilistic imputation based off the proportion of seasons for each cause in `CAUSE.CATEGORY`(from rows whose `MONTH` or `OUTAGE.START.DATE` was not missing).

In addition to this, Alaska and Hawaii are not assigned any U.S. Climate Regions, so we assigned the climate regions `Northern Temperate` and `Tropics` to them, respectively.

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

CLIMATE.REGION is NMAR. All outages in Alaska and Hawaii are missing CLIMATE.REGION because they are not a part of the continental United States, and therefore have no U.S Climate Region, so the null values in the CLIMATE.REGION column are dependent on the fact that Alaska and Hawaii have no U.S. Climate Region.

### Missingness Dependency: MAR vs MCAR Imputation Tests




<iframe src="resources/missingness-cdfs.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Test

### Hypotheses

### Testing

### Results

### Discussion







