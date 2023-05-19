# Icy-Hot Power

*Authors: Penelope King, Garvey Li*


## Introduction

### The Question and the Dataset

(!) still have to add why dataset and question is important to readers

Is the distribution of outages across U.S. climate regions the same in both winter and summer?


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

Focusing on univariate analysis, we created a graph to see what the distribution of seasons looked like in our DataFrame via a bar graph. Looking at our graph, we noticed that there was a high number of summer and winter power outages that occurred in our dataset. It seems that power outages recorded happen a lot during the winter and summer months.

<iframe src="resources/univariate_1.html" width=800 height=600 frameBorder=0></iframe>

We also looked at the distribution for different causes for power outages in our dataset. Looking at our bar graph, a high count of our data is represented in severe weather. The majority of the data recorded in our dataset seems to be power outages caused by severe weather. The second highest cause of power outages seems to be intentional attacks.

<iframe src="resources/univariate_2.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

After looking at different trends in individual columns of the dataset, we moved onto bivariate analysis to see what interactions between columns were happening in our data. Specifically we looked at what the distribution of seasons were for each cause for power outage in CAUSE.CATEGORY. We noticed that summer was highly represented in public appeal and equipment failure. Winter was the highest proportion of outages caused by fuel supply emergencies.

<iframe src="resources/bivar_1.html" width=800 height=600 frameBorder=0></iframe>

We also wanted to see which regions of the US had more winter caused accidents rather than summer caused accidents. To achieve this we created a choropleth which showed the proportion of winter caused power outages out of the total number of winter and summer power outages per state. We did this by filtering the data frame to have only winter and summer seasons and then grouping by postal code. Then we applied a lambda function to get the proportion of winter power outages out of winter and summer power outages. The darker the color on the map, the more winter power outages a state has compared to summer power outages. Before making the graph we thought that areas with more extreme weather in the winter may be expressing higher proportions of winter power outages. Looking at the graph, areas of the US known to have worse winters do seem to have darker proportions of winter caused power outages on our choropleth, but this is not an absolute rule.

<iframe src="resources/chloro.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Aggregating the data in different ways in table format also allows us to have a better understanding of our data. Specifically we wanted to see what were the most common occurrences for power outages by CLIMATE.REGION. We looked at this to better understand if climate regions had a large difference in the representation of different months, seasons, states, and causes. Looking at the grouped table we created, we noticed that the modes of all the climate regions were limited to summer and winter seasons. Furthermore, the most represented cause categories in these various climate categories were severe weather, intentional attack, and islanding. This makes sense, since summer and winter are known to have more extreme weather than spring and fall. 

<iframe src="resources/groupby1.html" width=800 height=600 frameBorder=0></iframe>

We also created a pivot table that looked at the distribution between summer and winter outages and the respective climate region that the outage occurred in. There seems to be some differences in some of the climate regions when it comes to if summer or winter has more power outages. We know that the US is known to have various climate regions with very different seasonal experiences. Summer and winter in particular are both seasons with extreme weather, but differ greatly between which area of the US a person is in. Would differences in weather affect the number of power outages between the two seasons? 

<iframe src="resources/pivot.html" width=800 height=600 frameBorder=0></iframe>


## Assessment of Missingness

### NMAR Analysis

The column CLIMATE.REGION is NMAR. All outages in Alaska and Hawaii are missing CLIMATE.REGION because they are not a part of the continental United States, and therefore have no U.S Climate Region. So, the null values in the CLIMATE.REGION column are dependent on the fact that Alaska and Hawaii have no U.S. Climate Region(It is not missing by design since the CLIMATE.REGION of Hawaii or Alaska cannot be inferred from any other columns, since the value does not exist). 

### Missingness Dependency: MAR vs MCAR Imputation Tests

For our analysis of missingness, we decided to look at the missingness of `CAUSE.CATEGORY.DETAIL`. 30.7% of this column is missing values, so its missingness is not non-trivial.



**YEAR**

The first column we decided to analyze the missingness of `CAUSE.CATEGORY.DETAIL` was `YEAR`, and our hypotheses are as follows:

Null Hypothesis: The missingness of `CAUSE.CATEGORY.DETAIL` does not depend on `YEAR` with a significance level of 0.05.

Alternative Hypothesis: The missingness of `CAUSE.CATEGORY.DETAIL` does depend on `YEAR` with a significance level of 0.05.

We calculated the TVD of the proportions of missing and not missing details per year and got an observed TVD of 0.2775. Looking at two distributions for missing and not missing, it seems as though they are extremely different. Which might suggest that the missingness of `CAUSE.CATEGORY.DETAIL` relies on the values of `YEAR`.

<iframe src="resources/det_year_missing.html" width=800 height=600 frameBorder=0></iframe>

After running 100,000 permutations on the `CAUSE.CATEGORY.DETAIL` column and computing the TVDs between missing and not missing for each `YEAR`, we got a p-value of 0, meaning that none of the permutations resulted in a TVD as large as the observed TVD.

<iframe src="resources/det_year_missing_test.html" width=800 height=600 frameBorder=0></iframe>

Since our p-value (0.0) is lower than our significance level (0.05), we reject the null hypothesis, so it is possible that the missingness of `CAUSE.CATEGORY.DETAIL` depends on `YEAR` so the missingness is MAR in relation to `CAUSE.CATEGORY.DETAIL`. 



**OUTAGE.START.TIME**


Null Hypothesis: The missingness of `CAUSE.CATEGORY.DETAIL` does not depend on `OUTAGE.START.TIME`

Alternative Hypothesis: The missingness of `CAUSE.CATEGORY.DETAIL` does depend on `OUTAGE.START.TIME`

<iframe src="resources/det_time_missing_cdf.html" width=800 height=600 frameBorder=0></iframe>



## Hypothesis Test
Finally for the hypothesis test. 
During the bivariate analysis we noticed that there seemed to be a difference between if a power outage would be more likely to occur during summer or winter depending on the climate region a person lived in. Could this be due to random chance?
Specifically, we are testing the question: Does climate region power outage distribution across winter and summer seasons differ? 

We set up our experiment like this:

### Hypotheses
#### Null Hypothesis:
In the population, the distribution of power outages for US climate regions are the same between summer and winter, and the observed differences in our sample are due to random chance.
#### Alternative Hypothesis:
In the population, the distribution of power outages of US climate region groups are different for summer and winter.

### Testing
Our test statistic was total variation distance (TVD) because we were looking at two different categorical distributions and how they differed from each other.
We set our significance level to the standard level of 0.05. That is, we are aiming for a 95% confidence level. For this experiment we used a permutation test because we are testing to see if two different sample distributions come from the same population distribution.

### Results and Discussion
Aftering running the permutation test, our p-value came to be around 0.0016.

Our p-value was less than our set significance level (0.0016 < 0.05), so we reject the null in favor of the alternative hypothesis. It is possible that in the US, a person can experience differing levels of summer or winter power outages based on what U.S. climate region they live in.


<iframe src="resources/bar-cause-cat.html" width=800 height=600 frameBorder=0></iframe>


