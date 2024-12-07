# league-of-legends-analysis
Step 1: Introduction

The dataset used for this project documents major power outage events across the United States. It includes various dimensions such as year, state, climate information, outage causes, outage duration, and the number of affected customers. 
Our question is the one is trying to predict the duration of the outrage when first signs of outrage appears: like complaint calls and warnings from the power grid. At this time, most of the detailed analysis are not availiable and we only have few info about the outrage: basically time, location and cliamte; we will use them to make an initial estimate of our outrage duration

OUTAGE.START.DATE: start date of the outrage
OUTAGE.START.TIME: start time of the outrage
U.S._STATE: state the outrage happens
NERC.REGION: grid nerc region where the outrage happens
CLIMATE.REGION: climate region of the outrage location
CLIMATE.CATEGORY: climate type of the outrage location
ANOMALY.LEVEL: climate anomaly level of the outrage
OUTAGE.DURATION: we are trying to predict the duration of the outrage with our initial guess

Our initial dataframe have 8 rows and 1534 columns.

Step 2: Data Cleaning and Exploratory Data Analysis
Data Cleaning
We perform the data cleaning and feature enginnering and convert the timestampe as sperarate year, month and date and hour. We do not use the time stamp directly and we extract more detailed info about them : YEAR, OUTAGE.START.MONTH,DAY_TYPE, OUTAGE.START. These newly added columns will be explained in the section below.
We drop the NA columns since most of the NA columns are from the HURRICANE.NAMES column, which is outside the scope of our columns of interest. We only lost 21 rows during the DropNA process, and that sample size is insignificant to our total sample size.












hello world
大家发一欧阿斯顿发看大家发建行卡发j
<iframe src="assets/yearly_power_outages.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/power_outages_by_state.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/power_outages_by_climate_and_state.html" width="800" height="600" frameborder="0"></iframe>
