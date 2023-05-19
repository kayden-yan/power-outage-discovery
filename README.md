# Power Outage Discovery
by Jiaqing Yan(j6yan@ucsd.edu), Shiyuan Wang(shw088@ucsd.edu)

---

## Introduction
This dataset encompasses information regarding significant power outages that occurred within the continental United States between January 2000 and July 2016, containing columns such as month, year, location, regional climate information, outage events, duration outage, electricity consumption, regional economic and land-use characteristics, etc. There are a few questions we might be interested in. Is it more likely to outage at night than morning? Are there any relationships between months and causes of the major power outages? Will it affect more and more customers as the outage lasts longer? Are there any relationships in the residential sector between monthly electricity prices and consumption? From these questions, the one we plan to investigate further is to find whether outages at night are more likely than morning.

---

## Cleaning and EDA

### Data Cleaning

Combined 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into a new pd.Timestamp column called 'OUTAGE.START'. 

| MONTH | POSTAL.CODE | CAUSE.CATEGORY     | OUTAGE.START        | OUTAGE.DURATION | CUSTOMERS.AFFECTED | RES.PRICE | RES.SALES |
|------:|:------------|:-------------------|:--------------------|----------------:|-------------------:|----------:|----------:|
|     7 | MN          | severe weather     | 2011-07-01 17:00:00 |            3060 |              70000 |     11.6  |   2332915 |
|     5 | MN          | intentional attack | 2014-05-11 18:38:00 |               1 |                nan |     12.12 |   1586986 |
|    10 | MN          | severe weather     | 2010-10-26 20:00:00 |            3000 |              70000 |     10.87 |   1467293 |
|     6 | MN          | severe weather     | 2012-06-19 04:30:00 |            2550 |              68200 |     11.79 |   1851519 |
|     7 | MN          | severe weather     | 2015-07-18 02:00:00 |            1740 |             250000 |     13.07 |   2028875 |

### Univariate Analysis

Plot 1: Here is the distribution of column CAUSE.CATEGORY. This pie chart shows the proportion of each events casuing the major power outages.

<iframe src="assets/univariate_plot1.html" width=800 height=600 frameBorder=0></iframe>

Plot 2: firstly we filter the power outage happens time, it's in the morning, in the evening, or nither. Then draw the plot to show the counts of power outage happens time in each categorical bin using bars.

<iframe src="assets/univariate_plot2.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

Plot 1: Here is s scatter plot using 'RES.PRICE column as x-axis and 'RES.SALSES' column as y-axis, we can see different associationns between prices and sales for different states.

<iframe src="assets/bivariate_plot1.html" width=800 height=600 frameBorder=0></iframe>

Plot 2: a scatter plot using 'CUSTOMERS.AFFECTED' column as x-axis, 'OUTAGE.DURATION' column as y-axis.

<iframe src="assets/bivariate_plot2.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

This pivot table has the 'CAUSE.CATEGORY' as index, 'MONTH' as columns, performs the mean of 'RES.PRICE' on different cauing event in different month by using aggfunction mean.

| CAUSE.CATEGORY        |        1 |       2 |       3 |        4 |       5 |       6 |       7 |       8 |       9 |       10 |      11 |     12 |
|:----------------------|---------:|--------:|--------:|---------:|--------:|--------:|--------:|--------:|--------:|---------:|--------:|-------:|
| equipment failure     |  14.52   | 12.4125 | 10.3775 |   9.92   | 12.6017 | 11.3814 | 12.2233 | 10.9025 | 10.805  | nan      |  11.24  | 12.465 |
| fuel supply emergency |  12.3075 | 16.2156 | 14.91   |  13.18   | 16.825  | 14.04   | 16.0017 | 15.972  | 14.4267 | nan      |  11.075 | 14.25  |
| intentional attack    |  11.2942 | 11.5157 | 12.3824 |  11.6112 | 13.2767 | 12.4112 | 12.7337 | 10.9688 | 12.879  |  12.6629 |  13.13  | 10.82  |
| islanding             | nan      | 13.695  | 13.4967 |  13.88   |  9.69   | 13.259  | 15.2125 | 11.2025 | 12.4733 |  14.84   |  15.29  | 14.54  |
| public appeal         |  11.5    | 11.804  | 10.765  | nan      | 10.158  | 10.0731 | 11.1879 | 10.8156 | 10.61   |  13.195  | nan     | 11.055 |

---

## Assessment of Missingness

Permutation test: The missingness of CUSTOMERS.AFFECTED depends on OUTAGE.DURATION.

We use mean difference between missing and not missing as our test statistic.

Null Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED equals the mean outage duration of not missing CUSTOMERS.AFFECTED.

Alternative Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED is smaller than the mean outage duration of not missing CUSTOMERS.AFFECTED.

<iframe src="assets/missingness_plot1.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/missingness_plot2.html" width=800 height=600 frameBorder=0></iframe>

We reject our null hypothesis at 0.01 level, this proves that the 'CUSTOMERS.AFFECTED' is NMAR and depends on 'OUTAGE.DURATION'

---

## Hypothesis Testing

From our dataset, we want to learn whether it is easier to occur power outage in the evening than the morning. We separate Morning/Evening by 6 a.m.(inclusive) and 6 p.m.(exclusive).

We observed that the missingness of outage start time is Missing by Design as those events falls into specific categories that is not directly power outage. So we exclude those missing outage start time in our testing.

From our hypothesis, we assume that the distribution of power outage is equally distributed in morning and evening. We use hypothesis test to simulate our results by generating AM or PM by probability of 50/50.

Null Hypothesis: The power outage happens in the morning as likely as in the evening.

Alternative Hypothesis: The power outage is more likely to happens in the morning than in the evening.

Our test statisic is difference between number of outages in the morning and number of outages in the evening.

Categorize outage start time into AM/PM.

|   OBS | OUTAGE.START        | is_am   |
|------:|:--------------------|:--------|
|     1 | 2011-07-01 17:00:00 | True    |
|     2 | 2014-05-11 18:38:00 | False   |
|     3 | 2010-10-26 20:00:00 | False   |
|     4 | 2012-06-19 04:30:00 | False   |
|     5 | 2015-07-18 02:00:00 | False   |

Counts of how many outages started.

| is_am   |   OUTAGE.START |
|:--------|---------------:|
| False   |            533 |
| True    |            992 |

<iframe src="assets/hypothesis_plot1.html" width=800 height=600 frameBorder=0></iframe>

We reject our Null Hypothesis at 0.01 level, which means the power outage is more likely to happens in the morning than in the evening. 
