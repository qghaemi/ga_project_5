﻿





# Project 5:  Developing AQI prediction feature for Dyson 

![](https://github.com/qghaemi/ga_project_5/blob/main/images/5a1ac806f65d84088faf136b.png)

Authors: Artem Lukinov, Mackenzie Dowling, Q Ghaemi

### [](https://github.com/qghaemi/ga_project_5#Contents)Contents:

-   [Background](https://github.com/qghaemi/ga_project_5#Background) 
-  [Problem Statement](https://github.com/qghaemi/ga_project_5#Problem-Statement)
-   [Collecting the Data](https://github.com/qghaemi/ga_project_5#Collecting-the-Data)
-    [Data Dictionary](https://github.com/qghaemi/ga_project_5#Data-Dictionary)
-   [Data Cleaning](https://github.com/qghaemi/ga_project_5#Data-Cleaning)
-  [Initial EDA](https://github.com/qghaemi/ga_project_5#Initial-EDA)
-  [The Modeling Process](https://github.com/qghaemi/ga_project_5#The-Modeling-Process)
-   [Conclusions and Recommendations](https://github.com/qghaemi/ga_project_5#Conclusions-and-Recommendations)

## [](https://github.com/qghaemi/ga_project_5#File-structure)File Structure

**datasets** folder - all data points and a cleaned, consolidated data file
**images** folder - all images used in the project and presentation
**1_data_cleaning.ipynb** - data cleaning notebook
**2_eda.ipynb** - Exploratory Data Analysis
**3_sarima_model.ipynb** - SARIMA modeling process and interpretations
**4_ar_models.ipynb** - AR modeling process and interpretations
**5_arima_models.ipynb** - ARIMA modeling process and interpretations

Package requirements: 
- pandas
- numpy
- matplotlib
- seaborn
- datetime
- statsmodels
- folium
- plotly

## [](https://github.com/qghaemi/ga_project_5#Background)Background

Dyson has long invested in filtration technologies and more recently into research on air quality as a broader phenomenon. Dyson believes that the first step to solving the global air quality problem is awareness and empowerment – equipping global populations with local data about the air quality they are exposed to and empowering them to make educated decisions to reduce air pollution exposure. With this information individuals can make more meaningful and impactful decisions for their health.

The next era of air purification systems: **Dyson Pure Cool 007**.

This product aims to have a greater impact on their consumers by preemptively preparing for rises in air-quality in order to notify and prepare consumers with the proper purification filters and additives. The Dyson Pure Cool 007 will be a cutting-edge filtration system that empowers the consumer while acting as a built-in marketing initiative to increase Dyson product sales and increase global awareness of air quality levels.


## [](https://github.com/qghaemi/ga_project_5#Problem-Statement)Problem Statement

In order to integrate this technology into the Dyson Pure Cool 007 product we have analyzed the largest county in population and AQI index in California to create a model that forecasts AQI with the lowest error. This model will provide updates notifying consumers in the county 7 days out, should it forecast an AQI increase in order to help them better prepare for poor air quality.
 
## [](https://github.com/qghaemi/ga_project_5#Collecting-the-Data)Collecting the Data
The data was acquired from EPA's web portal and included county level daily measurements from 01-01-2009 to 10-31-2020 with files separated by year.  A function helped iterate through all files, extract information for California and compile it into a separate .CSV file.

## [](https://github.com/qghaemi/ga_project_5#Data-Dictionary)Data Dictionary
After collecting and cleaning the data, this was the final Data Dictionary:

| Column Name | Description |	Data type	|
|--|--|--|
| date | Date | DateTimeIndex |
| state_name | State Name| string |
| county_name | County Name | string |
| county_code | Unique code for county | int64 |
| aqi | AQI observed | int64 |
| category | AQI Category | string |
|defining_parameter|Parameter affecting AQI|string
| defining_site| ID of defining monitor | string
|number_of_sites_reporting | number of monitors used | int64

## [](https://github.com/qghaemi/ga_project_5#Data-Cleaning)Data Cleaning
After the final dataframe was concatenated from all files, a thorough review of all columns was performed to identify potential features for the model. The data was pretty clean with no null values. State Code was dropped as it was duplicated by State Name. 

## [](https://github.com/qghaemi/ga_project_5#Initial-EDA)Initial EDA

| County Name |Average AQI  |
|--|--|
| San Bernardino | 96.00 |
| Riverside | 95.25 |
| Kern | 89.18 |
| Los Angeles | 88.11 |
| Tulare | 85.82 |

The top 4 counties are connected by borders and are near Los Angeles that has one of the highest polluted air in the country. The number 5 spot belongs to Tulare county that has Sequoia National Forest which was subject to forest fires.

![ACF, quarter level](https://github.com/qghaemi/ga_project_5/blob/main/images/ACF_quarterly.png)
There is a seasonal cycle at every 4 lags (quarters) for MA(1). The big spike at lag1 for MA(1), if it was MA(2) we would see two spikes lag1 and lag2, repeated in a 4 quarter cycle.


![PACF, quarterly level](https://github.com/qghaemi/ga_project_5/blob/main/images/PACF_quarterly.png)
Very similar season pattern, except this tells AR also looks to be AR(1). We are seeing one big spike, then starts to converse towards zero (with a quarterly cycle).

Null Hypothesis testing. Null Hypothesis is that our data is non-stational. 

|       County    |         T-stat     |                 P-Value             |
|-----------------|------------------------|---------------------------|
| Fresno          | (-7.902655318187861,)  | (4.1453186773950825e-12,) |
| Mendocino       | (-8.988391489956772,)  | (6.999189738568435e-15,)  |
| Mono            | (-60.01966974218798,)  | (0.0,)                    |
| Inyo            | (-16.186964061995763,) | (4.2091326788322163e-29,) |
| Orange          | (-18.294491318228168,) | (2.2935748930225705e-30,) |
| Glenn           | (-16.650285463308983,) | (1.6062512633519064e-29,) |
| Napa            | (-14.129971946847455,) | (2.3430321136051895e-26,) |
| Kings           | (-8.036857680053524,)  | (1.892526885735924e-12,)  |
| Stanislaus      | (-8.82373764098362,)   | (1.8474008730705937e-14,) |
| Ventura         | (-9.65905677288847,)   | (1.3690539739720768e-16,) |
| Sacramento      | (-9.179547347022764,)  | (2.2714444881795982e-15,) |
| San Diego       | (-8.394280540312796,)  | (2.3216098426868725e-13,) |
| Santa Clara     | (-12.61114835173,)     | (1.6542442722440998e-23,) |
| San Luis Obispo | (-9.176803214231677,)  | (2.3084019156810897e-15,) |
| Merced          | (-8.840305650241133,)  | (1.6754642977843016e-14,) |
| Solano          | (-15.044413368844356,) | (9.490233766082425e-28,)  |
| Lake            | (-8.487138932560649,)  | (1.3437864902315261e-13,) |
| Shasta          | (-7.919722258733582,)  | (3.752455317587813e-12,)  |
| Tuolumne        | (-4.50203382108771,)   | (0.00019498264728916943,) |
| Sutter          | (-9.108750442944608,)  | (3.4451545335533884e-15,) |
| Placer          | (-5.816334578543635,)  | (4.2838506267244656e-07,) |
| Monterey        | (-15.319334831971933,) | (4.084434958410942e-28,)  |
| Tulare          | (-6.4030000039835375,) | (1.9742980977008052e-08,) |
| Butte           | (-8.682339179772388,)  | (4.2527139757933626e-14,) |
| Imperial        | (-7.10359304902002,)   | (4.1078020589669727e-10,) |
| Santa Barbara   | (-15.685329611767303,) | (1.4557948954990032e-28,) |
| El Dorado       | (-4.484301924495623,)  | (0.0002098897838279341,)  |
| San Joaquin     | (-8.391115796311484,)  | (2.3652571522170513e-13,) |
| Madera          | (-7.442745213454148,)  | (5.946423491711733e-11,)  |
| Amador          | (-4.414856952572634,)  | (0.0002793683273024364,)  |
| Kern            | (-7.307803647346963,)  | (1.2877487459414348e-10,) |
| San Bernardino  | (-4.718815320471751,)  | (7.756428726856198e-05,)  |
| Sonoma          | (-14.46291051182916,)  | (6.80066960488543e-27,)   |
| Colusa          | (-6.375847686524517,)  | (2.2849439618108637e-08,) |
| San Francisco   | (-8.367266386559054,)  | (2.721627675265487e-13,)  |
| Contra Costa    | (-17.15934804676276,)  | (6.858423829229356e-30,)  |
| Del Norte       | (-4.070420089968666,)  | (0.0010834967721230053,)  |
| Calaveras       | (-5.8108554438975,)    | (4.404984529489785e-07,)  |
| Siskiyou        | (-6.568363332297761,)  | (8.051818466096985e-09,)  |
| Plumas          | (-6.205845609796767,)  | (5.660609449958287e-08,)  |
| San Mateo       | (-11.252019433646629,) | (1.7027802384066468e-20,) |
| Santa Cruz      | (-7.666030431406716,)  | (1.640321842067484e-11,)  |
| Mariposa        | (-7.420806957248976,)  | (6.744449238825392e-11,)  |
| Riverside       | (-5.0625666753648835,) | (1.6663561139595642e-05,) |
| Nevada          | (-5.890745725748906,)  | (2.928486145983533e-07,)  |
| San Benito      | (-7.040412994211131,)  | (5.8665996771406e-10,)    |
| Humboldt        | (-6.563539009701867,)  | (8.266640876801781e-09,)  |
| Yolo            | (-9.928208913334066,)  | (2.869080500872283e-17,)  |
| Los Angeles     | (-6.569448651522044,)  | (8.00425410486587e-09,)   |
| Tehama          | (-6.64613127223816,)   | (5.260083991892482e-09,)  |
| Alameda         | (-18.559357480026556,) | (2.0882310706885387e-30,) |
| Trinity         | (-4.681968991684861,)  | (9.096687320851715e-05,)  |
| Alpine          | (-3.365505086844199,)  | (0.012192536290753503,)   |


## [](https://github.com/qghaemi/ga_project_5#The-Modeling-Process)The Modeling Process

## [](https://github.com/qghaemi/ga_project_5#Conclusions-and-Recommendations)Conclusions and Recommendations


