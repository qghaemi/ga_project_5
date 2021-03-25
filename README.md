





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

- **datasets** folder - all data points and a cleaned, consolidated data file
- **images** folder - all images used in the project and presentation
- **1_data_cleaning.ipynb** - data cleaning notebook
- **2_eda.ipynb** - Exploratory Data Analysis
- **3_sarima_model.ipynb** - SARIMA modeling process and interpretations
- **4_ar_models.ipynb** - AR modeling process and interpretations
- **5_arima_models.ipynb** - ARIMA modeling process and interpretations

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

Null Hypothesis testing. Null Hypothesis is that our data is non-stationary. 

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

Based on p-values being significantly low, we can safely reject the Null hypothesis and determine that our data is stationary. This gives validity to use a model with autoregressive forecasting to predict the AQI score.

![ACF, quarter level](https://github.com/qghaemi/ga_project_5/blob/main/images/ACF_quarterly.png)

There is a seasonal cycle at every 4 lags (quarters) for MA(1). The big spike at lag1 for MA(1), if it was MA(2) we would see two spikes lag1 and lag2, repeated in a 4 quarter cycle. This confirms that we need to incorporate moving average into our model.


![PACF, quarterly level](https://github.com/qghaemi/ga_project_5/blob/main/images/PACF_quarterly.png)

Very similar seasonal pattern, except this tells AR also looks to be AR(1). We are seeing one big spike, then starts to converse towards zero (with a quarterly cycle).

These steps confirm that we also need to add a seasonality component into our model.

## [](https://github.com/qghaemi/ga_project_5#The-Modeling-Process)The Modeling Process

Based on the confirmations found in EDA, we believed our forecasting model would require seasonality (SARIMA), moving average (ARIMA), and auto-regression (AR). Each of the models selected have at least one of these features to explore forecasting with. We chose to use root mean squared error (RMSE) across all models in the same time window in order to determine the strongest model.

The time period we chose was all forecasts from the start of the data until the end of 2018. Then our testing data was the full year 2019. We chose not to forecast for the full-year 2020 as we did not have data for the full year and saw that the COVID lockdowns played an impact on the AQI scores for the year thus potentially introducing outliers into the model just in the testing data.

We chose to use RMSE as the measurement for each model as we found AIC scores to be less reliable across different model types. As we were most focused on the predicted values (forecast) measuring model strength by the RMSE of the predictions seemed more suited for this project. We developed a RMSE that would measure only over a set number of prediction, allowing RMSE for the first seven predictions as well as RMSE for all predicted values.

![SARIMA predictions](https://github.com/qghaemi/ga_project_5/blob/main/images/sarima_fc.png)

SARIMA scores:
- 33155.47 AIC Score
- RMSE 7-day: 17.09
- Full RMSE: 31.90

The red line in the graph represents the expected future data based on the forecasting model we built. The red line represents the average forecasted value for each day, and we would not be surprised to see the actual numbers track to this line for the most part. But there is no guarantee of this!

The gray area above and below the red line represents the 95 percent confidence interval and as with virtually all forecasting models, as the predictions go further into the future, the less confidence we have in our values. In this case, we are 95 percent confident that the actual AQI will fall inside this range. But, there is a chance the actuals could fall completely outside this range also. The larger the future time period for which we want to predict, the larger this confidence range will be (that is, the less precise our forecast is).

![AR predictions](https://github.com/qghaemi/ga_project_5/blob/main/images/LA_preds.png)

AR Scores:
- 6.28 AIC Score
- RMSE 7-day: 16.70
- Full RMSE: 36.42

We chose to explore AR models as it will calculate the regression of past AQI scores and calculate the predicted AQI score. We want to be considerate of previous AQI scores.

After reviewing quarterly PACF a seasonal cycled was observed which confirmed AR to be a significant parameter for our model. Based on where the lag occurred, the baseline value for AR should be AR(1).


![ARIMA predictions](https://github.com/qghaemi/ga_project_5/blob/main/images/ARIMA_preds.png)

ARIMA scores:
- 30113.33 AIC Score
- RMSE 7-day: 19.15
- Full RMSE: 37.49

A baseline ARIMA model which we compared to the AR model that we built. We wanted to build baseline models to determine which would return our lowest AIC score before deciding to adjust the parameters of a specific model.

We chose to build an ARIMA model because our forecasting window was very small and this is the strength of an ARIMA model which led us to believe this model could be the one we would adjust.

## [](https://github.com/qghaemi/ga_project_5#Conclusions-and-Recommendations)Conclusions and Recommendations

|Model  | AIC | RMSE 7-day| RMSE Full|
|--|--|--|--|
|AR| 6.28 |16.70|36.42|
|ARIMA|30113.33|19.15|37.49|
|SARIMA|33155.47|17.09|31.90|

Although AR model's AIC score is the lowest by a large margin, we are disregarding the AIC scores in favor of RMSE. Since the task is to forecast 7 days out, we have evaluated all of the models by their respective 7-day RMSE and Full RMSE scores.

AR scored the best in 7-day RMSE score. However, SARIMA model's score difference was negligible and its Full RMSE score was significantly lower.

Recommendations:
1. Tune SARIMA model for best RMSE by using Grid Search on a more powerful CPU and GPU. 
2. Build out individual models for each county in California.
3. Build out individual models for each county in the country.
