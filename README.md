# Data Analysis for Power Outages


## REPORT
Power Outages dataset because we thought it provides a comprehensive view of various aspects of power outages, allowing for detailed data analysis including time series analysis, handling missing values, and correcting data inconsistencies. We thought this analyis was important because we did find very interesting differences amongst various climate regions at different times of the year which can have significant implications for infrastructure planning and disaster preparedness. We focused mostly on weather and climate regions on power outages especially customers impacted by these power outages. Power outages are a common and often disruptive event that can have significant economic and social impacts. By analyzing how weather conditions and climate regions affect power outages, we can provide valuable insights that help in planning and resource allocation to reduce the adverse effects on customers. Our Dataset has 1534 columns and 55 columns where each row resembled each power outage. 
### Relevant Columns:
CUSTOMERS.AFFECTED: The number of customers impacted by each power outage. This is a key metric for quantifying the impact of power outages.
CLIMATE.REGION: The climate region where the outage occurred. Different regions may have varying levels of exposure to weather conditions that affect power outages.
ANOMALY.LEVEL: The level of anomaly in weather conditions, indicating unusual weather patterns that might contribute to power outages.
MONTH: The month in which the outage occurred. This helps in analyzing seasonal patterns in power outages.
YEAR: The year in which the outage occurred, useful for identifying trends over time.
CAUSE.CATEGORY: The general category of the cause of the outage, such as weather, equipment failure, or human error.
CAUSE.CATEGORY.DETAIL: More specific details about the cause of the outage within the general category.
TOTAL.CUSTOMERS: The total number of customers served by the utility company in the affected area. This helps in understanding the scale of the outage in relation to the customer base.


## DATA CLEANING AND EXPLORATORY DATA ANALYSIS

We perfomed data cleaning on OUTAGE.START.TIME, OUTAGE.START.DATE, OUTAGE.RESTORATION.TIME, OUTAGE.RESTORATION.DATE, CLIMATE.REGION, CUSTOMERS.AFFECTED, ANOMALY.LEVELS, and CLIMATE.CATEGORY. Each cleaning method is described below:

### OUTAGE.START columns and OUTAGE.RESTORATION columns
We did a bunch of cleaning for the outage start data, outage start time, outage restoration data and time columns so we have all the information situated in 1 column for both start and restoration. We did so by changing it to datetime first and then to timedelta and then adding both the start data and time to have them in the same column. As well, we filled in the 9 NAN values in the OUTAGE.START and OUTAGE.RESTORATION columns in two different ways: filling in the NANs with the average duration of power outages + their start timestamp in OUTAGE.START and filling in OUTAGE.START and OUTAGE.RESTORATION by imputing the mean start and end times per region, which is the CLIMATE.REGION column. 

### CLIMATE.REGION and CUSTOMERS.AFFECTED
There were a total of 4 NANs in the CLIMATE.REGION column for the states Hawaii and Alaska. For these values, we manually filled Hawaii with a postal code of HI and Alaska with AK. We imputated the null values in the CUSTOMERS.AFFECTED column by imputing with their average customers affected by the state the power outage took place. 

### ANOMALY.LEVELS and CLIMATE.CATEGORY
We filled the null values in ANOMALY.LEVEL using probabilistic imputation conditioned on the MONTH and YEAR columns of our dataframe. There were still 9 null values in the dataframe because the anomaly levels of those month and year were filled with NaN. Since the anomaly levels were values from known values of the oceanic El Niño/La Niña index referring to the cold and warm episodes by season that were also averaged over periods of 3 months, we took the values that attributed to the month and year the remaining null values and probabilistically imputed the anomaly levels manually. 

After this imputation, we realized the CLIMATE.CATEGORY column had 9 NaN values. Since the oceanic index table also color coded the values for each group of 3 months as warm, cold, or normal (which are the values used in CLIMATE.CATEGORY which is pulled from the same table as anomaly values) we manually filled in the climate categories using month and year to get the category. The last data cleaning step we performed was removing Alaska's power outage from the dataframe because there was only 1 reported outage where most of the values were NaN and nothing to reference to perform imputations. 

### HEAD OF CLEANED DATAFRAME

|   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |     2.30874e+06 |          276286 |           10673 |       2.5957e+06  |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |  5.34812e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |               124007 |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |     2.34586e+06 |          284978 |            9898 |       2.64074e+06 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |  5.45712e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |     2.30029e+06 |          276463 |           10150 |       2.58690e+06 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |  5.3109e+06  |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |     2.31734e+06 |          278466 |           11010 |       2.60681e+06 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |  5.38044e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |     2.37467e+06 |          289044 |            9812 |       2.67353e+06 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |  5.48959e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |




## UNIVARIATE ANALYSIS
We saw the distribution of the CLIMATE.CATEGORY column of our data frame and how many of each category we have. 

<iframe
  src="assets/fig7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above bar plot, most of the power outages reported in the dataframe occured in normal climate, with a total reported of 744 counts of outages in normal climate, whereas 473 cases happened in cold climate and 308 happened in hot climate. This trend of having more outages in a normal climate category shows how weather might not be a leading cause in causing power outages and there may be other factors that impact the occurrence of a power outage. 

As well, we plotted the distribution of CLIMATE.REGIONS in our data frame to see how many power outages were reported to occur in each region. The distribution is plotted below:

<iframe
  src="assets/fig5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In the above plot, most of the power outages seem to occur in the Northeast climate region, which includes Maryland, Pennsylvania, New Jersey, District of Columbia,
Delaware, New York, Vermont, Connecticut, Massachusetts, Maine and New Hampshire. More outages could be reported in this region because the states are densly populated or because more relevant outages were recorded in these states in comparison to others. As well, there could be other factors within this climate regions that make it more prone to power outages than other regions.


## BIVARIATE ANALYSIS
For bivariate analysis we made a histogram looking at the distribution of cause catogory across each climate region and found that severe weather was the most likely cause affecting a power outage.

<iframe
  src="assets/fig1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## INTERESTING AGGREGATES

| CAUSE.CATEGORY                |      1.0 |      2.0 |      3.0 |      4.0 |      5.0 |      6.0 |      7.0 |      8.0 |      9.0 |     10.0 |     11.0 |      12.0 |
|:------------------------------|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|---------:|----------:|
| equipment failure             |  77455.2 | 259779   | 108138   |  92702.7 | 159632   |  82336.8 |  69725.5 | 182649   | 294398   |    nan   |  83399.7 | 130684    |
| fuel supply emergency         | 128283   | 131868   | 105834   | 156825   | 196021   | 153510   | 150654   | 155919   | 165116   |    nan   | 194949   | 159285    |
| intentional attack            |  55724.6 |  57833.8 |  78141.8 |  35017.9 |  33483.4 |  49386.3 |  43819.8 |  52123.6 |  36363.6 |  26950.6 |  17482.3 |  53145.1  |
| islanding                     |    nan   |  11465.5 |  82893.9 |      0.5 |  70152.4 |  27946   |  25624.5 |  27314.8 |  11533.3 |   1714   |  18493.7 |   4783.33 |
| public appeal                 | 102859   | 182306   | 175397   |    nan   |  80546.3 |  90043.7 | 133452   | 113218   | 149429   | 212299   |    nan   | 117933    |
| severe weather                | 228473   | 164264   | 125699   | 141429   | 167916   | 162943   | 136524   | 191218   | 300866   | 251450   | 134434   | 194949    |
| system operability disruption | 106823   | 191257   | 178195   | 130680   | 267161   |  78130.3 |  87644.9 | 637573   | 193158   | 135028   | 182500   |  99959.7  |


The pivot table displayed above provides a detailed view of the average number of customers affected by different cause categories across each month.
We thought it was imporant to make this pivot table because by analyzing the average number of affected customers per cause category, stakeholders can identify patterns and trends over time. For example, certain months might consistently show higher averages for specific causes, indicating a seasonal trend. This did help us which months had what cause affect the customers the most. As well, it gives us more insight on what types of power outages affect more customers. For example. the highest value in this pivot table is 99959.7, showing that the highest average number of customers that were affected was due to system operability disruptions that occured in the month of December. These month distributions can also shed some insight on which power outages seem to happen more frequently than others in a certain time of the year. 



## ASSESMENT OF MISSINGNESS
We do think the column CUSTOMERS AFFECTED is NMAR because we did some analysis and found out that customers affected was most missing for cause category of intentional attack and we thought that could be so, 

We investigate the missingness of anomaly level values and climate region and we achieved a p-value of 



We investigated the missingness of the values of CAUSE.CATEGORY and CAUSE.CATEGORY.DETAIL because when we examined the missingness of the details, we saw that more values were missing when the cause of the power outage was severe weather. The graph below shows the distribution of the cause being severe weather and the counts of the detail being missing or not missing. 

<iframe
  src="assets/fig10.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We performed a permutation test to examine the missingness of cause category and cause category detail. The graph below shows the distribution of the TVDs calculated from our permutation test for determining the missingness of the cause of the power outage vs the details of the cause of the power outage (Cause Category vs Cause Category Detail). From the graph and our permutation test, we received a p-value of 0.0, meaning that the missingness of cause category detail is very dependent on cause category. 

<iframe
  src="assets/fig26.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We performed a permutation test to examine the missingness of ANOMALY.LEVEL and CLIMATE.REGION. The graph below shows the distribution of the TVDs calculated from our permutation test for determining the missingness of the anomaly level of that respecttive power outage vs the climate region in which the outage occured (ANOMALY.LEVEL vs CLIMATE.REGION). From the graph and our permutation test, we received a p-value of 0.0, meaning that the missingness of climate region is not very dependent on anomaly level.

## HYPOTHESIS TESTING
1. Null Hypothesis: The same amount of customers were affected by power outages in normal and warm climate coniditons

2. Alternative Hypothesis: More customers were affected by power outages due to normal climate conditions than warm climate conditions

### Choice of Test Statistic
The test statistic chosen is the difference in mean number of customers affected between regions with a warm climate and those with a normal climate. This is a suitable choice because we are interested in comparing the means of two groups to determine if there is a significant difference.


### SIGNIFICANCE LEVEL
We chose a significance level (alpha) of 0.05. 
### Obserevd Statistic
6034.653

### P-VALUE
0.3159
Based on our p-value, if the p-value is less than the significance level (0.05), we reject the null hypothesis. This means there is significant evidence to suggest that the average number of customers affected by outages is different between regions with normal and warm climates. Otherwise, we fail to reject the null hypothesis, indicating that any observed difference could be due to random chance.



## FRAMING A PREDICTION PROBLEM
Predicting the number of customers affected by power outages due cause categories, specific month, and climate region. 


### Type: Regression
Understanding and predicting the number of customers affected by power outages is crucial for resource allocation, emergency response planning, and minimizing the impact on communities. By accurately predicting this number, utilities and emergency management agencies can better prepare for and mitigate the effects of severe weather events.


### Evaluation Metric

Mean Absolute Error (MAE)

Reason for Choice: MAE is chosen because it provides a straightforward measure of prediction error by averaging the absolute differences between the predicted and actual values. It is less sensitive to outliers compared to metrics like Mean Squared Error (MSE) and provides a clear interpretation of the average error magnitude. This is important for practical applications where understanding the typical deviation from actual values is critical for planning and decision-making.


## BASELINE MODEL
### Model: RandomForestRegressor

Algorithm: RandomForestRegressor with 100 estimators and a random state set to 0 for reproducibility.

### FEATUES ADDED :

CLIMATE.REGION: This feature represents the climate region where the outage occurred. Different climate regions may have varying levels of exposure to weather conditions that can affect power outages. For example, regions prone to hurricanes or snowstorms might experience more severe outages affecting more customers. Catergorical feature as it is a nominal feature

CAUSE.CATEGORY: This feature categorizes the cause of the outage, such as weather, equipment failure, or human error. Understanding the cause can provide insight into the severity and impact of the outage on customers. This was our Catergorical feature as it is a nominal feature


MONTH: This feature represents the month in which the outage occurred. Outages may have seasonal patterns, with certain times of the year (like winter or hurricane season) potentially leading to more severe outages. This was our numerical feature


Categorical Features (Nominal): CLIMATE.REGION and CAUSE.CATEGORY were encoded using OneHotEncoder, which converts categorical variables into a series of binary columns (one-hot encoding). This method handles categorical data without assuming any ordinal relationship.


Numerical Features (Quantitative): MONTH was passed through without any transformation since it is already numerical.


The Mean Absolute Error on the test set was 145663.421.


### MODEL EVALUATION:
The feature set is really low and may not accurately represent all the elements that actually effect the CUSTOMERS.AFFECTED column. Overall, while the baseline model gave us an idea as to how all the features had an effect on the customers affected and a reasonable MAE, it needs to be made better, such as adding more relevant features and performing hyperparameter tuning and that could potentially made possible improve its performance. The inclusion of additional features related to outage conditions, infrastructure, and demographics could provide a more comprehensive understanding of the factors affecting customer outages.

## FINAL MODEL

### FEATURES ADDED :
ANOMALY.LEVEL: This feature measures the level of abnormal conditions (e.g., extreme temperatures or unusual weather patterns). Higher anomaly levels may correlate with more significant outages due to extreme weather events.

TOTAL.CUSTOMERS: This feature represents the total number of customers served in the area affected by the outage. More populated areas may have more complex infrastructure, leading to larger outages when problems occur. So we thought this would have show correlation to the customers affected itself. For example, an outage in a densely populated urban area will likely impact more customers than one in a sparsely populated rural area, even if the outage duration and cause are similar.

POSTAL.CODE: This feature captures the specific area affected by the outage. Including this feature allows the model to account for localized factors that may influence outage severity, such as local infrastructure quality or specific environmental conditions.

OUTAGE.DURATION: This feature measures the duration of the outage. Longer outages likely affect more customers, making it a crucial predictor for the number of customers affected.

### Hyperparameter Tuning Method:
The hyperparameters were selected using GridSearchCV, which performs an exhaustive search over specified parameter values. The model was evaluated using 5-fold cross-validation with the scoring metric set to negative mean absolute error (neg_mean_absolute_error).

### Mean Absolute error:
87811.706
### Best Parametres - 
max_depth - 20, min_samples_split - 10, n_estimators - 300
### Improvement from Baseline to Final

## FAIRNESS ANALYSIS

### Choice of Groups
Group X: Customers in areas with the climate category "normal"
Group Y: Customers in areas with the climate category "warm"
### Evaluation Metric
Mean Absolute Error (MAE): This metric measures the average magnitude of the errors in predictions, without considering their direction. It is a common evaluation metric for regression models as it provides a clear interpretation of prediction accuracy.

### Hypotheses
Null Hypothesis (H0): The model's prediction errors (MAE) for Group X (normal climate) and Group Y (warm climate) are the same. In other words, any observed difference in MAE is due to random chance.

Alternative Hypothesis (H1): The model's prediction errors (MAE) for Group X and Group Y are different. In other words, the observed difference in MAE is statistically significant, indicating potential unfairness.

### Test Statistic
The difference in MAE between the two groups (Group X and Group Y). That is difference of MAE between customers in areas with climate category as 'normal' and 'warm'. 
Observed MAE Difference - -713.862
### Significance Value 
Alpha : 0.05

### P-value
0.5539
### Conclusion
P-value is greater than or equal to the significance level, we fail to reject the null hypothesis, concluding that the model is fair. This means that there is insufficient evidence to conclude that the model's prediction errors (MAE) for Group X (normal climate) and Group Y (warm climate) are statistically significantly different. This implies that the model does not systematically favor or disadvantage either group based on the climate category. 
