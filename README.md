Data Analysis for Power Outages Project.
REPORT
We chose the power outages dataset because we thought it gives us real time exposure to different types of data analysis like time series, handling different Null values and wrongly represented data. We thought this analyis was important because we did find very interesting differneces amongst various climate regions at different times of the year which we further analyzed. We mostly focused mostly on weather and climate regions on power outages especially customers affected by the power outages. Our Dataset has 1534 columns and 55 columns where each row resembled each power outage. The columns we used up the most were CUSTOMERS.AFFECTED, CLIMATE.REGION, ANOMALY.LEVEL, MONTH, YEAR, CAUSE.CATEGORY, CAUSE.CATERGORY.DETAIL, TOTAL.CUSTOMERS and some more columns. We thought columns measuring customers affected was a good metric to quantify the effect each power outage and has and how it varies with across other featues in the dataset. 

DATA CLEANING AND EXPLORATORY DATA ANALYSIS
We did a bunch of cleaning for the outage statr data, outage start time, outage restoration data and time columns so we have all fo that in 1 column for both start and restoration. We did so by changing it to datetime first and then to timedelta and then adding both the start data and time to have themin the same column. We also filled in for the climate region which were nans for some states and regions using the postal code column. We also imputated a lot for customers affected by looking at the average customers affected within that state, month by doing dt.month over the outage.start column of the dataframe etc. We had to manually fill in for the anomaly levels and the cilmate category columns by looking at the cold and warm episodes by season table and looking at the year and the month where the respective columns have missing values and fill in for them. 

HEAD OF CLEANED DF

UNIVARIATE ANALYSIS
We saw the distribution of the CLIMATE.CATEGORY column of our data frame and how many of each category we have. We found that there were more of normal and then cold while warm has the least number of values. We also made a bar plot for each climate.region and found the below:

As obvious, there were most of northeast climate region and least of Alaska. 

BIVARIATE ANALYSIS
For bivariate analysis we made a histogram looking at the distribution of cause catogory across each climate region and founc that severe weather was the most likely cause affecting a power outage.

We 

ASSESMENT OF MISSINGNESS
The pivot table displayed below provides a detailed view of the average number of customers affected by different cause categories across each month. 


We thought it was imporant to make this pivot table because by analyzing the average number of affected customers per cause category, stakeholders can identify patterns and trends over time. For example, certain months might consistently show higher averages for specific causes, indicating a seasonal trend. This did help us which months had what cause affect the customers the most. 

We do think the column CUSTOMERS AFFECTED is NMAR because we did some analysis and found out that customers affected was most missing for cause category of intentional attack and we thought that could be so, 

HYPOTHESIS TESTING
1. Null Hypothesis: The same amount of customers were affected by power outages in normal and warm climate coniditons

2. Alternative Hypothesis: More customers were affected by power outages due to normal climate conditions than warm climate conditions

CHOICE of test statistic
The test statistic chosen is the difference in mean number of customers affected between regions with a warm climate and those with a normal climate. This is a suitable choice because we are interested in comparing the means of two groups to determine if there is a significant difference.

SIGNIFICANCE LEVEL
We chose a significance level (alpha) of 0.05. 
P-VALUE
0.473
Based on our p-value, if the p-value is less than the significance level (0.05), we reject the null hypothesis. This means there is significant evidence to suggest that the average number of customers affected by outages is different between regions with normal and warm climates. Otherwise, we fail to reject the null hypothesis, indicating that any observed difference could be due to random chance.

FRAMING A PREDICTION PROBLEM
Predicting the number of customers affected by power outages due cause categories, specific month, and climate region. 

Type: Regression
Understanding and predicting the number of customers affected by power outages is crucial for resource allocation, emergency response planning, and minimizing the impact on communities. By accurately predicting this number, utilities and emergency management agencies can better prepare for and mitigate the effects of severe weather events.


Evaluation Metric

Metric: Mean Absolute Error (MAE)

Reason for Choice: MAE is chosen because it provides a straightforward measure of prediction error by averaging the absolute differences between the predicted and actual values. It is less sensitive to outliers compared to metrics like Mean Squared Error (MSE) and provides a clear interpretation of the average error magnitude. This is important for practical applications where understanding the typical deviation from actual values is critical for planning and decision-making.

BASELINE MODEL
FEATUES ADDED :
CLIMATE.REGION: This feature represents the climate region where the outage occurred. Different climate regions may have varying levels of exposure to weather conditions that can affect power outages. For example, regions prone to hurricanes or snowstorms might experience more severe outages affecting more customers. Catergorical feature as it is a nominal feature
CAUSE.CATEGORY: This feature categorizes the cause of the outage, such as weather, equipment failure, or human error. Understanding the cause can provide insight into the severity and impact of the outage on customers. This was our Catergorical feature as it is a nominal feature
MONTH: This feature represents the month in which the outage occurred. Outages may have seasonal patterns, with certain times of the year (like winter or hurricane season) potentially leading to more severe outages. This was our numerical feature
Categorical Features (Nominal): CLIMATE.REGION and CAUSE.CATEGORY were encoded using OneHotEncoder, which converts categorical variables into a series of binary columns (one-hot encoding). This method handles categorical data without assuming any ordinal relationship.
Numerical Features (Quantitative): MONTH was passed through without any transformation since it is already numerical.
The Mean Absolute Error on the test set was 107960.
Looking at the evaluation of the model 


FINAL MODEL
