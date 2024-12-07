# league-of-legends-analysis
Introduction
The dataset used for this project documents major power outage events across the United States. It includes various dimensions such as the year, state, climate information, outage causes, outage duration, and the number of affected customers.

This project focuses on predicting the duration of a power outage when the first signs of an outage, such as complaint calls or warnings from the power grid, are detected. At this early stage, detailed information is often unavailable, and only basic attributes like time, location, and climate conditions are known. We aim to utilize this limited information to make an initial estimate of the outage duration, providing crucial insights for emergency response and resource allocation.

The key columns relevant to this prediction task are as follows:

OUTAGE.START.DATE: The date when the outage began.
OUTAGE.START.TIME: The time when the outage began.
U.S._STATE: The state where the outage occurred.
NERC.REGION: The NERC grid region associated with the outage.
CLIMATE.REGION: The climate region of the outage location.
CLIMATE.CATEGORY: The climate type of the outage location.
ANOMALY.LEVEL: The level of climate anomaly in the outage location.
OUTAGE.DURATION: The target variable, representing the duration of the outage.
This analysis will explore patterns in the dataset, build predictive models, and evaluate their performance to provide actionable insights.

Our initial dataframe have 8 rows and 1534 columns.

1.Data Cleaning
We performed data cleaning and feature engineering to prepare the dataset for analysis:

Feature Engineering:

The OUTAGE.START.DATE and OUTAGE.START.TIME columns were split into separate components, such as YEAR, OUTAGE.START.MONTH, and OUTAGE.START.HOUR. This allows for a more granular analysis of the temporal patterns of outages.
A new column, DAY_TYPE, was added to categorize outages based on whether they occurred on a weekday or weekend. This feature was included to analyze potential behavioral or systemic patterns in outage durations.
Handling Missing Values:

The HURRICANE.NAMES column contained missing values that were outside the scope of our analysis. Rows with missing values were dropped, resulting in the removal of 21 rows out of the entire dataset. This small loss of data is statistically insignificant and does not impact the overall quality of the dataset.
Overview of Cleaned Data:

After cleaning, the dataset includes the following relevant columns:
YEAR: The year the outage occurred.
OUTAGE.START.MONTH: The month when the outage began.
DAY_TYPE: Whether the outage started on a weekday or weekend.
CLIMATE.CATEGORY: The type of climate where the outage occurred.
ANOMALY.LEVEL: The level of climate anomaly at the outage location.
OUTAGE.DURATION: The duration of the outage, which is the target variable for prediction.

2.Univariate Analysis
To better understand the distribution of key variables, we performed univariate analyses on the following columns:

<iframe src="assets/yearly_power_outages.html" width="800" height="600" frameborder="0"></iframe>

<iframe src="assets/power_outages_by_state.html" width="800" height="600" frameborder="0"></iframe>

















<iframe src="assets/Power_Outages_by_Climate_Category_and_State.html" width="800" height="600" frameborder="0"></iframe>