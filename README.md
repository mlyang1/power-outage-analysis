# power-outages
DSC 80 Project 3

# Where and when do major power outages tend to occur?
by Michael Yang

---

## Introduction

This dataset contains data on major power outages in the continental United States from January 2000 to July 2016. Major power outages are defined as those that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 megawatts. Geographical location, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages are also included in the dataset.

My analysis is centered around the question: Where and when do major power outages tend to occur?

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

##### Regional Electricity Consumption Information
Data is per U.S. State
* TOTAL.PRICE : Average monthly electricity price (in cents/kilowatt-hour)
* TOTAL.SALES : Total electricity consumption (in megawatt-hours)
* TOTAL.CUSTOMERS : Annual number of total customers served

---

## Cleaning and EDA

### Data Cleaning

To clean the dataset and ensure the best possible representation of the underlying data generating process (DGP), I modified and combined certain columns to aid in analysis.

The "OUTAGE.START.TIME" string column was converted into a pandas datetime object and combined with the "OUTAGE.START.DATE" datetime column to create a single "OUTAGE.START" pd.Timestamp column. The same process was done for the "OUTAGE.RESTORATION" family of columns.

Missing values of various types were automatically filled in with nan and NaT values upon reading in the Excel file.

The 'MONTH' column, read in as a float column, was changed to int.
Through analysis, 9 rows with missing date information were determined to be incomplete repeats of existing rows, and were removed.

The unit of OUTAGE.DURATION was converted to days.

|   YEAR |   MONTH | STATE     | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED | CLIMATE.REGION     | CLIMATE.CATEGORY   |   TOTAL.PRICE |   TOTAL.SALES |   TOTAL.CUSTOMERS |
|-------:|--------:|:----------|:--------------------|:---------------------|------------------:|:-------------------|:------------------------|-----------------:|---------------------:|:-------------------|:-------------------|--------------:|--------------:|------------------:|
|   2011 |       7 | Minnesota | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |       2.125       | severe weather     | nan                     |                0 |                70000 | East North Central | normal             |          9.28 |   6.56252e+06 |           2595696 |
|   2014 |       5 | Minnesota | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |       0.000694444 | intentional attack | vandalism               |                0 |                    0 | East North Central | normal             |          9.28 |   5.28423e+06 |           2640737 |
|   2010 |      10 | Minnesota | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |       2.08333     | severe weather     | heavy wind              |                0 |                70000 | East North Central | cold               |          8.15 |   5.22212e+06 |           2586905 |
|   2012 |       6 | Minnesota | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |       1.77083     | severe weather     | thunderstorm            |                0 |                68200 | East North Central | normal             |          9.19 |   5.78706e+06 |           2606813 |
|   2015 |       7 | Minnesota | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |       1.20833     | severe weather     | nan                     |              250 |               250000 | East North Central | warm               |         10.43 |   5.97034e+06 |           2673531 |


### Univariate analysis

<iframe src="assets/year_hist.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows that the number of major power outages in the continental U.S. steadily increased from year 2000 to 2010, before exploding to a peak in 2011, and then decreasing at a faster rate from 2011 to 2016. The peak in 2011 is correlated with a cold climate category, indicating oceanic La Niña conditions.

### Bivariate Analysis

<iframe src="assets/date_duration_scatter.html" width=800 height=600 frameBorder=0></iframe>

This scatter plot shows the relationship between outage start dates and outage duration in days. Overall, duration of outages seems to stay about the same from 2000 to 2016, but longer outages than before have begun appearing since in 2009.

### Interesting Aggregates

| CAUSE.CATEGORY                |   2000 |   2001 |   2002 |   2003 |   2004 |   2005 |   2006 |   2007 |   2008 |   2009 |   2010 |   2011 |   2012 |   2013 |   2014 |   2015 |   2016 |
|:------------------------------|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|
| equipment failure             |      2 |      1 |      0 |      6 |      5 |      3 |      1 |      6 |      9 |     10 |      5 |      4 |      1 |      4 |      0 |      0 |      0 |
| fuel supply emergency         |      0 |      0 |      0 |      0 |      0 |      1 |      0 |      3 |      4 |      3 |      3 |      6 |      5 |      7 |     13 |      2 |      3 |
| intentional attack            |      2 |      0 |      1 |      2 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |    121 |     89 |     80 |     47 |     43 |     33 |
| islanding                     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      1 |      6 |      4 |      9 |      3 |      4 |      7 |      1 |      9 |      2 |
| public appeal                 |      0 |      3 |      0 |      1 |      7 |      0 |      2 |      2 |      2 |     10 |     14 |     16 |      3 |      0 |      5 |      4 |      0 |
| severe weather                |     10 |      1 |     12 |     30 |     56 |     47 |     54 |     40 |     76 |     45 |     62 |    107 |     65 |     49 |     45 |     48 |     12 |
| system operability disruption |      5 |     10 |      3 |      7 |      3 |      4 |      9 |      4 |     14 |      6 |     13 |     12 |      7 |      6 |      1 |     13 |      9 |

This pivot table shows the number of outages for each cause category and year.
From the table, I can see that severe weather remains the most consistent cause for a high number of major power outages each year. I can also observe the unusual trend that no outages were caused by intentional attacks until 2011, when intentional attacks were the cause of 121 outages. Then from 2011 onward, dozens of outages each year are from intentional attacks.

---

## Assessment of Missingness

### NMAR Analysis

I believe the "DEMAND.LOSS.MW" column is not missing at random (NMAR). As stated in the dataset's documentation, this column contains misreported data (reporting total demand instead of amount of peak demand lost during outage event). Because of this, I cannot determine the values of missing data in this column from other columns in the dataset, without knowledge of the data generating process.

Data I might want to obtain that could explain this missingness could be a separate column containing total demand for each observation, such that instances where total demand is reported in the DEMAND.LOSS.MW column can be identified. Missing values in DEMAND.LOSS.MW could then be compared with the new total demand column to see if it is MAR.

### Missingness Dependency

Using missingness permutation tests, the missingness of data in the 'OUTAGE.RESTORATION' column was found to be MAR dependent on the 'CAUSE.CATEGORY' column.

<iframe src="assets/dependency.html" width=800 height=600 frameBorder=0></iframe>

This plot visualizes the difference between the distributions of outage causes when the date of outage restoration is missing and not missing. It appears that 'OUTAGE.RESTORATION' contains more missing data for the cause category 'fuel supply emergency', and less missing data for the cause category "severe weather". This suggests I can assume the value of missing outage restoration data is MCAR within the cause category.

---

## Hypothesis Testing

The question I am trying to answer is: "Where and when do major power outages tend to occur?" My null hypothesis is that major power outages occur equally across the continental United States. My alternative hypothesis is that major power outages occur more often in certain states than others. My choice of test statistic was the TVD because I will be using the categorical 'STATE', 'YEAR', and 'MONTH' columns to determine the time of outage occurrence in each state. I used the 95th percentile, 0.00479, as the significance level. The resulting p-value was 0.01. Because the p-value is greater than the 95th percentile of the empirical distribution of the TVD, I reject the null hypothesis. Therefore, it is unlikely that major power outages occur equally across the continental United States.



---
