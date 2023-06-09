# Power Outage Discovery

by Jiaqing Yan(j6yan@ucsd.edu), Shiyuan Wang(shw088@ucsd.edu)

The data came from [here](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks).

---

## Introduction
This dataset encompasses information regarding significant power outages that occurred within the continental United States between January 2000 and July 2016, containing columns such as month, year, location, regional climate information, outage events, duration outage, electricity consumption, regional economic and land-use characteristics, etc. The question we analyze is centered around how we identify patterns in power outages in order to better prevent them. The data presented is containing 55 variables.

The dataset and our question have the potential to draw readers’ attention because we live in a world where people reply so much on electricity that power outage could bring great potential inconvenience to their daily life, and have an data-based insight on which time frame (morning or evening) in a day power outages tend to occur more frequently could potentially help them lessen the impact that power outages might bring to their life. 

There are 1540 rows in the dataset, or 1535 valid rows if we exclude the 5 rows that contain information about the columns, such as names and units of the columnes. The columns relevant to our question is “OUTAGE.START.TIME” and “OUTAGE.START.DATE,” which represent the start time of the power outages in a day and the start date of the power outages, respectively. These two columns were later combined into one column for our use: “OUTAGE.START,” which contains the time and date of the stars of the power outages. 

---

## Cleaning and EDA

### Data Cleaning

We took 3 major steps in cleaning the data.
First step: We dropped the first 4 invalid rows, which contain information about columns instead of actual data. The 5th row contains the correct column names, so we set it as the names of columns in the data frame and then dropped this row.

Second step: We combined “OUTAGE.START.TIME” and “OUTAGE.START.DATE” into a single column. We achieved this through zipping the two columns into a single pandas series and then applied our predefined lambda function on each entry of the series, converting each entry into a pandas timestamp object by calling the constructor of pd.Timestamp. Finally, we set this series as a new combined column named “OUTAGE.START,” and then dropped “OUTAGE.START.TIME” and “OUTAGE.START.DATE” columns. 

Third step: Similar to the second step, we combined “OUTAGE.RESTORATION.TIME” and “OUTAGE.RESTORATION.DATE” into a single column. We achieved this through zipping the two columns into a single pandas series and then applied our predefined lambda function on each entry of the series. We set this series as a new combined column named “OUTAGE.RESTORATION,” and then dropped “OUTAGE.RESTORATION.TIME” and “OUTAGE.RESTORATION.DATE” columns. 


| MONTH | POSTAL.CODE | CAUSE.CATEGORY     | OUTAGE.START        | OUTAGE.DURATION | CUSTOMERS.AFFECTED | RES.PRICE | RES.SALES |
|------:|:------------|:-------------------|:--------------------|----------------:|-------------------:|----------:|----------:|
|     7 | MN          | severe weather     | 2011-07-01 17:00:00 |            3060 |              70000 |     11.6  |   2332915 |
|     5 | MN          | intentional attack | 2014-05-11 18:38:00 |               1 |                nan |     12.12 |   1586986 |
|    10 | MN          | severe weather     | 2010-10-26 20:00:00 |            3000 |              70000 |     10.87 |   1467293 |
|     6 | MN          | severe weather     | 2012-06-19 04:30:00 |            2550 |              68200 |     11.79 |   1851519 |
|     7 | MN          | severe weather     | 2015-07-18 02:00:00 |            1740 |             250000 |     13.07 |   2028875 |

### Univariate Analysis

A univariate analysis we performed was on the reasons for power outages. We created a pie chart (plot 1) showing the distribution of the reasons for power outages, finding out the frequent reason is severe weather, which accounts for 49.7% of power outages.

Plot 1: Here is the distribution of column CAUSE.CATEGORY. This pie chart shows the proportion of each events casuing the major power outages.

<iframe src="assets/univariate_plot1.html" width=800 height=600 frameBorder=0></iframe>

Plot 2: firstly we filter the power outage happens time, it's in the morning, in the evening, or nither. Then draw the plot to show the counts of power outage happens time in each categorical bin using bars.

<iframe src="assets/univariate_plot2.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

A bivariate analysis we performed was on the residential sector monthly electricity price versus their electricity consumption. We created a scatter plot where the residential sector monthly electricity price was on the x-axis, finding out that there seems to be an interesting trend that for most states, the higher the monthly electricity price, the more consumption.

Plot 1: Here is s scatter plot using 'RES.PRICE column as x-axis and 'RES.SALSES' column as y-axis, we can see different associationns between prices and sales for different states.

<iframe src="assets/bivariate_plot1.html" width=800 height=600 frameBorder=0></iframe>

Plot 2: a scatter plot using 'CUSTOMERS.AFFECTED' column as x-axis, 'OUTAGE.DURATION' column as y-axis.

<iframe src="assets/bivariate_plot2.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

This pivot table has the 'CAUSE.CATEGORY' as index, 'MONTH' as columns, performs the mean of 'OUTAGE.DURATION' on different cauing event in different month by using aggfunction mean.

| CAUSE.CATEGORY        |         1 |         2 |          3 |        4 |         5 |        6 |        7 |         8 |        9 |       10 |      11 |        12 |
|:----------------------|----------:|----------:|-----------:|---------:|----------:|---------:|---------:|----------:|---------:|---------:|--------:|----------:|
| equipment failure     |   618.667 |   479.75  | 19851.5    |  370.429 |   386.167 |  270.333 |  305.083 |   288.667 | 332.75   | nan      | 503     |   838.25  |
| fuel supply emergency | 31813.5   | 14435.6   | 17667.6    | 8468     | 18717     | 1890     | 5136     | 12410.8   | 865      | nan      | 758     | 15059     |
| intentional attack    |   303.617 |   372.474 |   977.263  |  781.878 |   365.295 |  142.088 |  426.061 |   166.615 | 434.333  | 214.059  | 237.333 |   620.609 |
| islanding             |   nan     |   359.5   |    51.6667 |  103.5   |   108.5   |  161     |  360.667 |   312.5   |  64.6667 |  13.3333 | 129     |   436     |
| public appeal         |  2681.25  |  1041.4   |   945.5    |  nan     |  2635     |  970.812 |  810.357 |  1673.78  |  30      | 351      | nan     |  6293.5   |

---

## Assessment of Missingness

From initial observation, we noticed that the OUTAGE.DURATION is either significantly shorter or longer when the CUSTOMERS.AFFECTED is missing. So we speculate that the extreme cases of OUTAGE.DURATION can leads to the missingness of CUSTOMERS.AFFECTED, such that CUSTOMERS.AFFECTED is NMAR.

We conduct permutation test to see if the missingness of CUSTOMERS.AFFECTED depends on OUTAGE.DURATION.

Null Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED equals the mean outage duration of not missing CUSTOMERS.AFFECTED.

Alternative Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED is smaller than the mean outage duration of not missing CUSTOMERS.AFFECTED.

We use mean difference between missing and not missing as our test statistic.

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

---

# Power Outage Prediction

---

## Framing the Problem
Our goal is to predict the duration of power outage based on features such as month, cause of the outage, anomaly level and start time, which are all information that could be accessed before the happening of each outage. Note that we can know the cause of the power outage by using many resources, for example, news reports regarding extreme weathers, the date of last time staff have maintained the equipment, or the live footage of people trespassing areas. The anomaly level can be estimated by the local climate conditions reported by reliable resources like weather reports. This is a regression problem. The response variable we choose is the duration of outage in minutes, because predicting outage duration could give people instructional information about approximately how much they need to wait before the power restores so that they could adjust their schedule accordingly to minimize the inconvenience brought by the outage. The metric we are using is rooted mean squared error (RMSE). We choose RMSE because it is a widely accepted metric with mathematical properties suitable for statistical analysis, and it provides direct information about the difference between predicted and actual data. We preferred RMSE over MSE because RMSE is on the same scale as the target variable compared to MSE, which makes interpreting the performance of the model more easy.

---

## Baseline Model

Our baseline model is a linear regression model that uses features of month and outage start time (in hour). Month and outrage start time are one-hot encoded categorical features in our case. For encodings, we converted the unit of start time to hour and then one-hot encoded it along with month. Our baseline model presented a training set RMSE of 4388.10 and testing set RMSE of 3695.96. It is worth noting that the training set RMSE is similar to testing set RMSE, suggesting that there is no over-fitting in our baseline model. We believe its performance is not decent enough, since the RMSE value is pretty high given that the average outage duration in the dataset is 2845.51.

<iframe src="assets/baseline_one.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/baseline_two.html" width=800 height=600 frameBorder=0></iframe>


---

## Final Model

For our final model, we engineered and added 2 new features – cause of the outage and anomaly level. Anomaly level represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season, and it could provide information about local climate conditions when the outages happen. Such climate information is useful since it typically takes longer to repair when the weather is extreme. Knowing what causes the outage could also be useful, since some causes of the outage often take longer for it to be repaired, such as severe weather and equipment failure. The modeling algorithm we chose is a random forest regressor, which combines multiple decision trees to perform regression tasks. The best hyperparameters are random_max_depth = 10 and randomforest_n_estimators = 50. The method we used to select the hyperparameters and the overall model is grid search with 5 folds. The final model performs better than the baseline model with an training RMSE of 2495.57, which is 1892,53 less than the baseline model. There might be some over-fitting in our final model because our testing RMSE is 3295.33, we tried many combinations of features and we conclude that the difference is due to the dataset doesn't contain a large enough or diversed enough dataset.

<iframe src="assets/final_model_one.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/final_model_two.html" width=800 height=600 frameBorder=0></iframe>

---

## Fairness Analysis

Our choice of group X and Y are outages in the mornings and outages in the evenings, respectively. Our evaluation metric is mean squared error. Our null hypothesis is that the performance of our model stays the same between morning and evening, while our alternative hypothesis is that the performance of our model is different between morning and evening. Our choice of test statistic is the absolute difference between the RMSE of the duration predictions of outages in the mornings and the RMSE of the ones in the evening. We choose significance level is 0.05. 

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

The resulting p-value is 0.468, so our conclusion is that we fail to reject the null hypothesis, which means that our model performance is consistent across morning and evening.


