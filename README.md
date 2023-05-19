# power-outage-discovery
---
## Introduction

---
## Cleaning and EDA

### Data Cleaning

Combined 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into a new pd.Timestamp column called 'OUTAGE.START'. Also, combined 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' into a new pd.Timestamp column called 'OUTAGE.RESTORATION'.

|   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

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

|   OBS | OUTAGE.START        | is_am   |
|------:|:--------------------|:--------|
|     1 | 2011-07-01 17:00:00 | True    |
|     2 | 2014-05-11 18:38:00 | False   |
|     3 | 2010-10-26 20:00:00 | False   |
|     4 | 2012-06-19 04:30:00 | False   |
|     5 | 2015-07-18 02:00:00 | False   |

| is_am   |   OUTAGE.START |
|:--------|---------------:|
| False   |            533 |
| True    |            992 |

<iframe src="assets/hypothesis_plot1.html" width=800 height=600 frameBorder=0></iframe>

We reject our Null Hypothesis at 0.01 level, which means the power outage is more likely to happens in the morning than in the evening. 
