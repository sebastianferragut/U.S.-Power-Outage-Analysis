# U.S. Power Outage Analysis
Power outage analysis using data from continental U.S. from January 2000 to July 2016. Created as part of DSC80 Final Project at UCSD.


# Introduction

This project involved analysis of a dataset with major power outages in the US from Jan 2000 - Jul 2016. The dataset was accessed through Purdue at https://engineering.purdue.edu/LASCI/research-data/outages.

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

"""Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

### Bivariate Analysis

"""Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

### Grouping and Aggregates

"""Embed at least one grouped table or pivot table in your website and explain its significance.


EMBED EXAMPLE 

<iframe
  src="assets/file-name.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Assessment of Missingness

## NMAR Analysis

"""State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”

## Missingness Dependency

### First category compared CHANGE THIS HEADER


### Second category compared CHANGE THIS HEADER

"""Present and interpret the results of your missingness permutation tests with respect to your data and question. Embed a plotly plot related to your missingness exploration; ideas include:
• The distribution of column 
Y
 when column 
X
 is missing and the distribution of column 
Y
 when column 
X
 is not missing, as was done in Lecture 8.
• The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.


# Hypothesis Testing

"""Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p
-value, and your conclusion. Justify why these choices are good choices for answering the question you are trying to answer.

Optional: Embed a visualization related to your hypothesis test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.

# Framing a Prediction Problem

"""Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).

Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your final exam grade, we couldn’t use your Final Project grade, because the project is only due after the final exam! Feel free to ask questions if you’re not sure.

# Baseline Model

"""Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.

Tip: Make sure to hit all of the points above: many projects in the past have lost points for not doing so.

# Final Model

"""State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

"""Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.


# Fairness Analysis

"""Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p-value, and your conclusion.

Optional: Embed a visualization related to your permutation test in your website.
