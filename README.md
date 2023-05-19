# power-outages
DSC 80 Project 3

# How have characteristics of major power outages changed over time?

by Michael Yang

---

## Introduction

This dataset contains data on major power outages in the continental United States from January 2000 to July 2016. Major power outages are defined as those that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 megawatts. Geographical location, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages are also included in the dataset.

Our analysis is centered around the question: How have characteristics of major power outages changed over time?

Analyzing historical trends and patterns of major power outages will provide insight into what factors may indicate greater risk of a major power outage in the United States.

Number of rows: 1534

#### Relevant Columns:

##### Outage Events Information
* OUTAGE.START : start date and time of power outage (as pandas datetime object)
* OUTAGE.RESTORATION : end date and time of power outage (as pandas datetime object)
* OUTAGE.DURATION : duration (in minutes)

* CAUSE.CATEGORY : category of event causing outage
* CAUSE.CATEGORY.DETAIL : more specific category of event causing outage

* DEMAND.LOSS.MW : amount of peak demand lost (in megawatts) [but in many cases, total demand is reported]
* CUSTOMERS.AFFECTED : Number of customers affected

##### Regional Climate Information
* CLIMATE.REGION : United States climate region as specified by National Centers for Environmental Information
* CLIMATE.CATEGORY : represents climate episode corresponding to year ('warm', 'cold', or 'normal' based on the Oceanic Niño Index (ONI))

##### Regional Electricity Consumption Information
Data is per U.S. State
* TOTAL.PRICE : Average monthly electricity price (in cents/kilowatt-hour)
* TOTAL.SALES : Total electricity consumption (in megawatt-hours)
* TOTAL.CUSTOMERS : Annual number of total customers served

---

## Cleaning and EDA

### Data Cleaning

To clean the dataset and ensure the best possible representation of the underlying data generating process (DGP), we modified and combined certain columns to aid in analysis.

The "OUTAGE.START.TIME" string column was converted into a pandas datetime object and combined with the "OUTAGE.START.DATE" datetime column to create a single "OUTAGE.START" pd.Timestamp column. The same process was done for the "OUTAGE.RESTORATION" family of columns.

Missing values of various types were automatically filled in with nan and NaT values upon reading in the Excel file.

The 'MONTH' column, read in as a float column, was changed to int.
To best represent the DGP, 9 rows with missing date information were removed.

The unit of OUTAGE.DURATION was converted to days.

|   YEAR |   MONTH | STATE     | OUTAGE.START        | OUTAGE.RESTORATION   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED | CLIMATE.REGION     | CLIMATE.CATEGORY   |   TOTAL.PRICE |   TOTAL.SALES |   TOTAL.CUSTOMERS |
|-------:|--------:|:----------|:--------------------|:---------------------|:-------------------|:------------------------|-----------------:|---------------------:|:-------------------|:-------------------|--------------:|--------------:|------------------:|
|   2011 |       7 | Minnesota | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | severe weather     | nan                     |              nan |                70000 | East North Central | normal             |          9.28 |   6.56252e+06 |           2595696 |
|   2014 |       5 | Minnesota | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | intentional attack | vandalism               |              nan |                  nan | East North Central | normal             |          9.28 |   5.28423e+06 |           2640737 |
|   2010 |      10 | Minnesota | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | severe weather     | heavy wind              |              nan |                70000 | East North Central | cold               |          8.15 |   5.22212e+06 |           2586905 |
|   2012 |       6 | Minnesota | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | severe weather     | thunderstorm            |              nan |                68200 | East North Central | normal             |          9.19 |   5.78706e+06 |           2606813 |
|   2015 |       7 | Minnesota | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | severe weather     | nan                     |              250 |               250000 | East North Central | warm               |         10.43 |   5.97034e+06 |           2673531 |          5.47874 |


### Univariate analysis

<iframe src="assets/year_hist.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows that the number of major power outages in the continental U.S. steadily increased from year 2000 to 2010, before exploding to a peak in 2011, and then decreasing at a faster rate from 2011 to 2016. The peak in 2011 is correlated with a cold climate category, indicating oceanic La Niña conditions.

### Bivariate Analysis

<iframe src="assets/date_duration_scatter.html" width=800 height=600 frameBorder=0></iframe>

This scatter plot shows the relationship between outage start dates and outage duration in days. Overall, duration of outages seems to stay about the same from 2000 to 2016, but longer outages than before have begun appearing since in 2009.


---

## Assessment of Missingness


---

## Hypothesis Testing


---
