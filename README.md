





# Project 5:  Developing AQI prediction feature for Dyson 
![](https://github.com/qghaemi/ga_project_5/blob/main/images/5a1ac806f65d84088faf136b.png)

Authors: Artem Lukinov, Mackenzie Dowling, Q Ghaemi

### [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Contents)Contents:

-   [Problem Statement](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Problem-Statement)
-   [Executive Summary](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Executive-Summary)
-   [Data Dictionary](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Data-Dictionary)
-   [Collecting the Data](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Collecting-the-Data)
-   [Data Cleaning](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Data-Cleaning)
-   [Pre-processing](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Pre-processing)
-  [Initial EDA](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Initial-EDA)
-  [The Modeling Process](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#The-Modeling-Process)
-   [Conclusions and Recommendations](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Conclusions-and-Recommendations)

    -   A succinct formulation of the question your analysis seeks to answer.
    -   A table of contents, which should indicate which notebook or scripts a stakeholder should start with, and a link to an  **executive summary**.
    -   A paragraph description of the data you used, plus your data acquisition, ingestion, and cleaning steps.
    -   A short description of software requirements (e.g.,  `Pandas`,  `Scikit-learn`) required by your analysis.

## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Problem-Statement)Problem Statement

Dyson’s Pure Cool division has been losing market share over the last year; to counteract this we have created a model that can be implemented into the latest Pure Cool product: an AQI forecaster. This forecaster will provide updates should it forecast AQI increasing from one category to the next in order to help users better prepare for declining air quality.

## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Executive-Summary)Executive Summary

Almost everyone has heard at least once about Star Wars and Star Trek universes. The Star Trek TV series was released in 1966 and was followed by Star Wars launch in 1977. Since then, countless episodes, movies, cartoons, games and other media have been released under both titles. Each universe has an almost cult following that has their own conventions, competitions and, of course, online presence. Two of those are on Reddit. Star Wars subreddit has 1.8m active members and Star Trek has over 300k trekkies on it. This project uses data from those subreddits to develop a Natural Language Processing model to identify which subreddit a certain post belongs to. The goal is to identify how similar or different those subreddits are.  
## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Data-Dictionary)Data Dictionary
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


## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Collecting-the-Data)Collecting the Data
The data was acquired from EPA's web portal and included county level daily measurements from 01-01-2009 to 10-31-2020 with files separated by year.  A function helped iterate through all files, extract information for California and compile it into a separate .CSV file.

## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Data-Cleaning)Data Cleaning
After the final dataframe was concatenated from all files, a thorough review of all columns was performed to identify potential features for the model. The data was pretty clean with no null values. State Code was dropped as it was duplicated by State Name. 


## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Pre-processing)Pre-processing



## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Initial-EDA)Initial EDA



## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#The-Modeling-Process)The Modeling Process

## [](https://github.com/ArtemLukinov/AQI-Prediciton-Tool#Conclusions-and-Recommendations)Conclusions and Recommendations


