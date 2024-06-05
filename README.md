# U.S. Power Outage Analysis
Power outage analysis using data from continental U.S. from January 2000 to July 2016. Created as part of DSC80 Final Project at UCSD.



# Introduction

This project involved analysis of a dataset with major power outages in the US from Jan 2000 - Jul 2016. The dataset was accessed through [Purdue](https://engineering.purdue.edu/LASCI/research-data/outages.). 

The dataset has a wide variety of information on the major outages, including but not limited to characteristics of the states in the continental U.S. such as climate and consumption patterns. 

The dataset will be cleaned to understand the data, then to analyze the missigness of the dataset. 

I will explore the research question: how do climate characteristics impact major power outages with high severity?

The original unmodified dataset has 1534 rows and 57 columns. 
For my analysis, I will use the following columns:


| Column | Description |
| ----------- | ----------- |
| CLIMATE.REGION | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.) |
| ANOMALY.LEVEL |  El Niño/La Niña (ONI) index referring to the cold and warm episodes by season |
| CLIMATE.CATEGORY | "Warm", "Cold" or "Normal" based on ONI |
| CAUSE.CATEGORY | Category of event causing major power outage |
| OUTAGE.START.DATE | Day of year when outage started |
| OUTAGE.START.TIME | Time of day when outage started |
| OUTAGE.RESTORATION.DATE | Day of year when outage stopped |
| OUTAGE.RESTORATION.TIME | Time of day when outage stopped |
| OUTAGE.DURATION | Duration of outage in minutes |
| CUSTOMERS.AFFECTED | Number of customers affected by power outage event |
| U.S._STATE | State where outage occurred |  



# Data Cleaning and Exploratory Data Analysis

## Cleaning 

These are the steps I took to clean up the dataset to prepare it for analysis:

1. I began by defining the rows and columns to use, then read in the Excel file using those arguments. The rows are all rows with data and the column row. 
2. I then combined the outage time columns and converted them to datetime objects. This involved converting the OUTAGE.START.DATE and OUTAGE.START.TIME to OUTATE.START, and OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME to OUTAGE.RESTORATION. I dropped these columns because the information is now condensed in OUTAGE.START and OUTAGE.RESTORATION to streamline analysis. 
3. I then cleaned the missing values and replaced them with np.NaN, including values that were stored in the original dataset as "NA".  
4. I finished cleaning by selecting only the relevant columns: CLIMATE.REGION, ANOMALY.LEVEL, CLIMATE.CATEGORY, CAUSE.CATEGORY, OUTAGE.START, OUTAGE.RESTORATION, OUTAGE.DURATION, CUSTOMERS.AFFECTED, and U.S._STATE. 

Below are a few rows and columns of the cleaned outage DataFrame. 

| CLIMATE.REGION     | CAUSE.CATEGORY     | OUTAGE.START        | OUTAGE.RESTORATION   |
|:-------------------|:-------------------|:--------------------|:---------------------|
| East North Central | severe weather     | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
| East North Central | intentional attack | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
| East North Central | severe weather     | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
| East North Central | severe weather     | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
| East North Central | severe weather     | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |  


## Exploratory Data Analysis 

### Univariate Analysis 

I first performed some univariate analysis to understand the distribution of a couple
individual variables. The most important of these analyses are shown below.

To start, I plotted the distribution of power outages by climate region, to understand which regions were most impacted. It revealed that the Northeast region was by far the most impacted region, followed by the South and West regions. 
<iframe
  src="assets/univariate1.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  

I then plotted the frequency of power outages by cause category, which shed some light on which types of causes were most common. Severe weather was the most common, followed by intentional attack.  
<iframe
  src="assets/univariate2.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  

### Bivariate Analysis

I then performed some bivariate analysis to understand the distribution between multiple variables. The most important of these analyses are shown below.

To start, I plotted OUTAGE.DURATION against CUSTOMERS.AFFECTED. The results were not very interpretable, but it did reveal that there was a concentration of short outages impacting a relatively small amount of customers. However, there was much variability and not a clear correlation here. Perhaps other relationships would be more enlightening. 
<iframe
  src="assets/bivariate1.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  

I then plotted OUTAGE.DURATION and CAUSE.CATEGORY. It revealed severe waether and fuel supply emergency as two leading causes in terms of outage duration.
<iframe
  src="assets/bivariate2.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  

### Grouping and Aggregates

I then grouped by CAUSE.CATEGORY with the aggregate function mean() to understand the average of OUTAGE.DURATION and CUSTOMERS.AFFECTED in relation to these cateogries. Here are the first several rows of this DataFrame: 

| CAUSE.CATEGORY                |   ('OUTAGE.DURATION', 'mean') |   ('CUSTOMERS.AFFECTED', 'mean') |
|:------------------------------|------------------------------:|---------------------------------:|
| equipment failure             |                      1816.91  |                        109223    |
| fuel supply emergency         |                     13484     |                             0.2  |
| intentional attack            |                       429.98  |                          1865.52 |
| islanding                     |                       200.545 |                          6169.09 |
| public appeal                 |                      1468.45  |                          7618.76 |
| severe weather                |                      3899.59  |                        188558    |
| system operability disruption |                       732.902 |                        212827    |    



# Assessment of Missingness

## NMAR Analysis

CUSTOMERS.AFFECTED might be NMAR because outages could be obstructing the systems responsible for data collection. In an outage situation, the priority may also be on restoring power, over ensuring reporting. 

Additional data that we may want to be able to explain the missingess could be reports on operations and infrastruture data, to determine if missing data is from technical failures or just operational strain. We may also want to see infrastructure data to understand the redundancy at play. It would be helpful to know what the standard operating procedure is under these circumstances. 

## Missingness Dependency
In order to test missingness dependency, I will compare the distriubution of OUTAGE.DURATION to CAUSE.CATEGORY and CLIMATE.REGION. To do so, I created a boolean column OUTAGE.DURATION.MISSING from OUTAGE.DURATION to analyze missigness. 

### CAUSE.CATEGORY

I first examined missingness in relation to CAUSE.CATEGORY. I performed a permutation test using the total variation distance. 

**Null Hypothesis**: The distribution of CAUSE.CATEGORY is the same when duration is missing vs not missing.

**Alternate Hypothesis**: The distribution of CAUSE.CATEGORY is different when duration is missing vs not missing.

Here we reject the null hypothesis with a p-value of 0.0 and an observed TVD of 0.2028. These indicate a difference in the distribution of CAUSE.CATEGORY when duration is missing versus not missing.

Below is the empirical distribtuion of the TVD for CAUSE.CATEGORY:

<iframe
  src="assets/causetvd.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>  



### CLIMATE.REGION

I then examined missingness in relation to CLIMATE.REGION. I performed a permutation test also using the total variation distance. 

**Null Hypothesis**: The distribution of CLIMATE.REGION is the same when duration is missing vs not missing.

**Alternate Hypothesis**: The distribution of CLIMATE.REGION is different when duration is missing vs not missing.

Here we fail to reject the null hypothesis with a p-value of 0.171 and an observed TVD of 0.0795. This indicates that there is not significant evidence to suggest a difference in the distribution of CLIMATE.REGION when duration is missing versus not missing.


Below is the empirical distribtuion of the TVD for CLIMATE.REGION:

<iframe
  src="assets/climatetvd.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>     



# Hypothesis Testing

As a reminder, the question I initially posed was:

How do climate characteristics impact major power outages with high severity?

**Null Hypothesis (H0)**: The mean outage duration is the same across different climate categories.

**Alternative Hypothesis (H1)**: The mean outage duration differs across different climate categories.

**Test Statistic**: Difference in means (difference between the maximum mean outage duration and the minimum mean outage duration across climate categories)

**Significance Level**: Significance level of 0.05. 

The test statistic was chosen as difference in means because the distributions of the features analyzed are appropriately similar. The resulting p-value was 0.766. With this p-value, we fail to reject the null hypothesis. This suggests that there is no significant evidence that climate categories have a significant impact on outage durations. 

Below is the empirical distriubtion of the test statistic: 

<iframe
  src="assets/climatehypo.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>    

We can also contextualize the failure to reject the null hypothesis by viewing the mean outage duration by climate category:

<iframe
  src="assets/climatemeanoutage.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>   


# Framing a Prediction Problem

My model will attempt to predict the cause of a major power outage. This is a multiclass classification problem. The response variable, CAUSE.CATEGORY, was chosen because it follows the research question: how do climate characteristics impact major power outages with high severity?

The metric I will use to evaluate my model will be F1 score, because it is a balance between accuracy and precision for imbalanced classes. 

At time of prediction, we would know climate region and customers affected through reporting and the outage zone, so we can use these as features to predict the target cause category.


# Baseline Model

The model I created is a multiclass Decision Tree classifier using the features climate region and customers affected to predict the cause category of power outages. Climate region is a categorical feature, while customers affected is a quantitative feature.

Of the 2 features, I handled the categorical feature with one hot encoding, and passed through the numerical feature. 

In terms of performance, the model is fair, with an accuracy score of 0.80 and an F1 score of 0.79. The F1 score is more important, as it accounts for imbalance in the classes. So, the model performs well much of the time, but it can still be improved with different features and classification techniques.



# Final Model

For the final model, I added month, year, and day of week. These features are good for the prediction task because seasonality and certain days of week can explain many outages, especially with regard to intentional attacks. 

The modeling algorithm I chose is a Random Forest classifier, which I analyzed using GridSearchCV with 5-fold cross validation. I chose this as an improvement over the Decision Tree classifier used in the baseline model. Using this method, it results that the hyperparameters that performed best were: entropy for criterion, a max depth of 20, min samples leaf set to 1, min samples split set to 3, and n estimators set to 50. 

The performance was a good improvement over the baseline model, with an accuracy score of 0.88, and an F1 score of 0.87. This means that the vast majority of the time, the model is correct in its predictions.



# Fairness Analysis

For fairness analysis, I decided to compare short duration outages to long duration outages, defined as short if below the mean of outage duration, and long if above the mean. The evaluation metric I used is the F1 score.

**Null Hypothesis (H0):** The model has a very similar F1 score for short outages and long outages, and differences are due to random chance.

**Alternative Hypothesis (H1):** The model does not have similar F1 scores for short outages and long outages, and differences are not by chance.

The test statistic I used is difference in group means for F1 score. The significance level I set for the p-value is 0.05. I ran a permutation test with 10,000 iterations, and the resulting p-value was 0.0, which suggests that the model's F1 score is different between short and long outages, so I reject the null hypothesis. 

Below is a graph of the distribution of the test statistic.

<iframe
  src="assets/fairnessperm.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>   