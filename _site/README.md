Data Analysis for Power Outages Project.


REPORT

We chose the power outages dataset because we thought it gives us real time exposure to different types of data analysis like time series, handling different Null values and wrongly represented data. We thought this analyis was important because we did find very interesting differneces amongst various climate regions at different times of the year which we further analyzed. We mostly focused mostly on weather and climate regions on power outages especially customers affected by the power outages. Our Dataset has 1534 columns and 55 columns where each row resembled each power outage. The columns we used up the most were CUSTOMERS.AFFECTED, CLIMATE.REGION, ANOMALY.LEVEL, MONTH, YEAR, CAUSE.CATEGORY, CAUSE.CATERGORY.DETAIL, TOTAL.CUSTOMERS and some more columns. We thought columns measuring customers affected was a good metric to quantify the effect each power outage and has and how it varies with across other featues in the dataset. 


DATA CLEANING AND EXPLORATORY DATA ANALYSIS

We did a bunch of cleaning for the outage statr data, outage start time, outage restoration data and time columns so we have all fo that in 1 column for both start and restoration. We also filled in for the climate region which were nans using the postal code column. We also imputated a lot for customers affected by looking at the average customers affected within that state, climate, month etc.

| U.S._STATE   | CLIMATE.CATEGORY   |   ANOMALY.LEVEL | OUTAGE.RESTORATION   | OUTAGE.START        |   MONTH |   YEAR |
|:-------------|:-------------------|----------------:|:---------------------|:--------------------|--------:|-------:|
| Minnesota    | normal             |            -0.3 | 2011-07-03 20:00:00  | 2011-07-01 17:00:00 |       7 |   2011 |
| Minnesota    | normal             |            -0.1 | 2014-05-11 18:39:00  | 2014-05-11 18:38:00 |       5 |   2014 |
| Minnesota    | cold               |            -1.5 | 2010-10-28 22:00:00  | 2010-10-26 20:00:00 |      10 |   2010 |
| Minnesota    | normal             |            -0.1 | 2012-06-20 23:00:00  | 2012-06-19 04:30:00 |       6 |   2012 |
| Minnesota    | warm               |             1.2 | 2015-07-19 07:00:00  | 2015-07-18 02:00:00 |       7 |   2015 |
