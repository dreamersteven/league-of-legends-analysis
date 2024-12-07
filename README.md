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

<iframe src="assets/Yearly Count of Major Power Outages.html" width="600" height="400" frameborder="0"></iframe>

<iframe src="assets/power_outages_per_state.html" width="600" height="400" frameborder="0"></iframe>






3. Bivariate Analysis
To identify relationships between pairs of variables, we analyzed the following combinations:


Extract the year from the OUTAGE.START.DATE and convert OUTAGE.DURATION to numeric

<iframe src="assets/Year vs Outage Duration.html" width="600" height="400" frameborder="0"></iframe>




Outage Duration by Climate Region

<iframe src="assets/Outage Duration by Climate Region.html" width="600" height="400" frameborder="0"></iframe>



it seems that month of the OUTAGE.START.TIME have some patterns with outrage duration
<iframe src="assets/Average Outage Duration by Month.html" width="600" height="400" frameborder="0"></iframe>




# Redefine the "DAY_TYPE" column based on the new weekday/weekend definition
now, we define weekday as Monday-Thursday and count Friday- Sunday as weekend
as we can see, there is a clear difference, because accidents that happens at the end of the week are more likelytake longer because less worker is available at weekends
<iframe src="assets/Average Outage Duration by Day Type (New Definition: Weekday vs Weekend).html" width="600" height="400" frameborder="0"></iframe>


# Extract the hour from the "OUTAGE.START.TIME" column and add it as a new column
#it seems that hour of the day in OUTAGE.START.TIME also effects the OUTAGE.DURATION
# accidents that happens in midnight - morning tend to recover much longer
<iframe src="assets/Average Outage Duration by Hour.html" width="600" height="400" frameborder="0"></iframe>



Average Outage Duration by U.S. State

<iframe src="assets/Average Outage Duration by U.S. State.html" width="600" height="400" frameborder="0"></iframe>



It seems that NERC region also affects the duration Calculate the average outage duration for each NERC region

<iframe src="assets/Average Outage Duration by NERC Region.html" width="600" height="400" frameborder="0"></iframe>


it seems that climate category are pretty unrelated to our dutation prediction

<iframe src="assets/Average Outage Duration by Climate Category.html" width="600" height="400" frameborder="0"></iframe>




4. Interesting Aggregates
We aggregated data to uncover significant patterns:

Average Outage Duration by State:


<iframe src="assets/Power_Outages_by_Climate_Category_and_State.html" width="600" height="400" frameborder="0"></iframe>


5. Imputation
Imputation was applied where necessary to handle missing values in key columns
We don’t have a lot of missing data


##  Framing a Prediction Problem
Our question is the one is trying to predict the duration of the outrage when first signs of outrage appears: like complaint calls and warnings from the power grid. At this time, most of the detailed analysis are not availiable and we only have few info about the outrage: basically time, location and cliamte; we will use them to make an initial estimate of our outrage duration

OUTAGE.START.DATE: start date of the outrage
OUTAGE.START.TIME: start time of the outrage
U.S._STATE: state the outrage happens
NERC.REGION: grid nerc region where the outrage happens
CLIMATE.REGION: climate region of the outrage location
CLIMATE.CATEGORY: climate type of the outrage location
ANOMALY.LEVEL: climate anomaly level of the outrage
OUTAGE.DURATION: we are trying to predict the duration of the outrage with our initial guess


The response variable is OUTAGE.DURATION, measured in minutes. This variable was chosen because understanding the duration of outages is critical for:

Improving response strategies and resource allocation during outages.
Enhancing the reliability and efficiency of power grid systems.
Supporting decision-making processes for utility companies and policymakers

This is a regression problem that aims to predict the `duration of power outages (OUTAGE.DURATION) based on various contextual, temporal, and geographic features. Since the target variable, OUTAGE.DURATION, is continuous, regression is the appropriate modeling approach.

Evaluation Metric:
The metric used to evaluate the model is Mean Absolute Error (MAE).

Why MAE?

Interpretability: MAE is expressed in the same units as the target variable, making it intuitive and easy to interpret (e.g., "average error in minutes").
Sensitivity to Scale: Unlike RMSE, MAE treats all errors equally, which is beneficial when we want to minimize the average error rather than disproportionately penalizing large deviations.
Robustness to Outliers: While MAE is not as sensitive to outliers as RMSE, it still provides a balanced measure for assessing model performance across the dataset.
Why Not RMSE or R²?

RMSE penalizes larger errors more heavily, which may not align with the goal of minimizing average error across all predictions.
R² is less intuitive for non-technical stakeholders and focuses on the proportion of variance explained, which is less actionable compared to direct error metrics like MAE.



## Baseline Model

Model Description:
The current model is a Random Forest Regressor implemented in a scikit-learn pipeline. It predicts outage duration (OUTAGE.DURATION) based on two features:

CLIMATE.CATEGORY (Categorical - Nominal)
ANOMALY.LEVEL (Numerical - Quantitative)

Feature Types:
Quantitative Features:
ANOMALY.LEVEL: A continuous numerical feature representing weather anomalies.
Ordinal Features:
None in this model, as no features with inherent order are included.
Nominal Features:
CLIMATE.CATEGORY: A categorical feature representing the type of climate, encoded as a one-hot vector.


The model shows reasonable performance for a simple implementation but has room for improvement:

Strengths:
The use of a Random Forest Regressor provides robustness and handles non-linear relationships effectively.
Preprocessing ensures the features are appropriately scaled and encoded for the model.
Weaknesses:
The MAE of ~2951 minutes suggests the model's predictions are moderately accurate but still deviate significantly in absolute terms.
The use of only two features limits the predictive power. Including additional relevant features (e.g., weather details, geographic location) could enhance performance.

The model shows reasonable performance for a simple implementation but has room for improvement:

Strengths:
The use of a Random Forest Regressor provides robustness and handles non-linear relationships effectively.
Preprocessing ensures the features are appropriately scaled and encoded for the model.
Weaknesses:
The MAE of ~3162 minutes suggests the model's predictions are moderately accurate but still deviate significantly in absolute terms.
The use of only two features limits the predictive power. Including additional relevant features (e.g., weather details, geographic location) could enhance performance.



##  Final Model


Features Added and Their Relevance to the Prediction Task:

U.S._STATE:
Why it’s relevant: Different states experience varying climatic conditions, energy grid infrastructures, and natural disaster risks (e.g., hurricanes, snowstorms). Including this feature helps the model account for regional variations in outage duration caused by state-specific factors.

NERC.REGION:
Why it’s relevant: NERC (North American Electric Reliability Corporation) regions define areas of grid management and reliability standards. Outage durations can depend on how efficiently regions manage and recover from outages, which makes this feature crucial.

CLIMATE.REGION:
Why it’s relevant: This feature captures broader climatic patterns (e.g., tropical, arid, temperate), which directly influence the frequency and severity of weather events leading to power outages.

CLIMATE.CATEGORY:
Why it’s relevant: Categories like “cold,” “normal,” and “warm” provide insight into specific climatic conditions during the outage, which can significantly impact restoration times. For instance, extreme cold might slow down recovery operations.

YEAR:
Why it’s relevant: Including the year allows the model to detect trends over time, such as improvements in grid infrastructure, evolving weather patterns, or policy changes that could affect outage durations.

OUTAGE.START.MONTH:
Why it’s relevant: Outages occurring in different months may correlate with seasonal weather patterns (e.g., hurricanes in fall, snowstorms in winter), providing a time-sensitive dimension to the model.

DAY_TYPE:
Why it’s relevant: Distinguishing between weekdays and weekends provides information about workforce availability and response times, which may vary between these periods.

OUTAGE.START.HOUR:
Why it’s relevant: The time of day when an outage starts can influence response times, as outages during nighttime hours may face delays in restoration compared to those during business hours.


The added features enhance the model’s understanding of contextual and temporal factors affecting outage duration. They align closely with real-world conditions influencing the outage response, such as geographic variability, infrastructure capabilities, and seasonality. By capturing these nuances, the model gains a more comprehensive view of the data-generating process, enabling it to make more accurate predictions.
Modeling Algorithm:

The Random Forest Regressor was chosen as the modeling algorithm for the final model. This algorithm is an ensemble method that combines multiple decision trees to improve predictive performance and robustness. It is well-suited to this problem because:

It can model non-linear relationships between the features and the target variable.
It handles both categorical and numerical data effectively.
It is robust to overfitting, particularly with an appropriate number of trees and depth.

Hyperparameters and Selection Method:
Best Hyperparameters:

n_estimators: Number of trees in the forest: 100
max_depth: Maximum depth of the trees: 10
min_samples_split: Minimum samples required to split an internal node: 5

Selection Method:

Grid Search Cross-Validation (GridSearchCV) was used to identify the best combination of hyperparameters. This method systematically tests multiple combinations of hyperparameters across several folds of the training data, evaluating performance using a cross-validated score (e.g., RMSE).
Reduced Parameter Grid: To balance computational efficiency and thoroughness, the parameter grid was limited to key hyperparameters (e.g., tree depth, number of estimators, minimum samples for splitting).

Final Model vs. Baseline Model Performance:

Final Model:

Mean Absolute Error (MAE): 2872.39 minutes
Key Improvements:
The final model used nine features, capturing both temporal and contextual aspects of the data.
Hyperparameter tuning optimized the Random Forest Regressor for better generalization.

Baseline Model:

Mean Absolute Error (MAE): 3162.38 minutes
Limitations:
The baseline model used only two features (CLIMATE.CATEGORY and ANOMALY.LEVEL), limiting its ability to capture complex relationships.
Default hyperparameters were used, which may not have been optimal for the problem.
