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

* CAUSE.CATEGORY : category of event causing outage
* CAUSE.CATEGORY.DETAIL : more specific category of event causing outage

* OUTAGE.DURATION : duration (in minutes)
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

|   YEAR |   MONTH | STATE     | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START        | OUTAGE.RESTORATION   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |
|-------:|--------:|:----------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:--------------------|:---------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|
|   2011 |       7 | Minnesota | MN            | MRO           | East North Central |            -0.3 | normal             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|   2014 |       5 | Minnesota | MN            | MRO           | East North Central |            -0.1 | normal             | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|   2010 |      10 | Minnesota | MN            | MRO           | East North Central |            -1.5 | cold               | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|   2012 |       6 | Minnesota | MN            | MRO           | East North Central |            -0.1 | normal             | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|   2015 |       7 | Minnesota | MN            | MRO           | East North Central |             1.2 | warm               | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |


### Univariate analysis

<iframe src="assets/year-hist.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---
