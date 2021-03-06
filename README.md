# Predicting Homeless Population Sizes 

Homelessness in America is a deepening crisis that is inextricably linked to poverty and systemic injustice. The current system to prevent and to house homeless individuals is inadequate and ill-equipped. In this project, I gathered publicly available data to build a predictive model that provides insight into homelessness population growth.

<p align="center"> <a href="http://www.youtube.com/watch?feature=player_embedded&v=7f9dqQBYjcA" target="_blank"><img src="http://img.youtube.com/vi/7f9dqQBYjcA/0.jpg" 
alt="myimage" width="500" height="380" border="10" /></a> </p> 

[*video from Princeton Eviction Lab](https://evictionlab.org/why-eviction-matters/#video)

The United States Department of Housing and Urban Development(HUD) gives grants to "Continuum of Care" applicants through an annual competition. A Continuum of Care (CoC) is a regional or local planning body that coordinates the funding for housing and homeless prevention services. CoCs distribute resources to nonprofits and local government programs across the regions they represent. The population size and geographic areas covered by each CoC are dramatically different across state lines. 

Two critical activities entrusted to CoCs are the biannual physical count of homeless people and an annual enumeration of transitional housing units and homeless shelter beds. These counts provide an overview of the state of homelessness in a CoC area and help HUD allocate resources. The counts do not reflect the people who do not wish to be seen or those who may have found shelter on that particular day. 

## Scope of Project: 
For this final project at the Flatiron School, I had 10 days to determine a topic/hypothesis, assemble the dataset, and develop an appropriate model. This was a self-directed project with weekly student check-ins. 

## Primary Data Sources | Years 2007:2016

[US Department of Housing and Urban Development: CoC Homeless Counts](https://www.hudexchange.info/resource/3031/pit-and-hic-data-since-2007/)

[US Social Security Administration: Supplemental Security Income Annual Reports](https://www.ssa.gov/policy/docs/statcomps/ssi_sc/2016/index.html) 

[US Department of Labor's Bureau of Labor Statistics: Unemployment Rates](https://www.kaggle.com/jayrav13/unemployment-by-county-us/data)

[Princeton University: Eviction Lab Statistics](https://data-downloads.evictionlab.org/)

[Kaiser Family Foundation: State Mental Health Agency Expenditures per Capita](https://www.kff.org/other/state-indicator/smha-expenditures-per-capita/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D)


[HUD Exchange: Continuum of Care Coverage](https://www.hudexchange.info/resource/4981/fy-2016-continuums-of-care-names-and-numbers/)
*(compiled the county list manually)*

## Compiling Data 

- Using python/pandas, annual HUD CoC files were converted into a single dataframe containing CoC number, year, homeless count. 

- Excel workbooks of annual Social Security data contained sheets for each state with spend itemized by county and demographic on each sheet. The data from 50 pages of each workbook were combined into dataframes, reformatted, and the year was added. The annual data was concatenated into one dataframe. 

- Unemployment data was already compiled via Kaggle. I grouped monthly observations into annual averages and reformatted the data.  

- Location data was compiled from three sources and merged into a dataframe with CoC number, FIPS code, latitude, longitude, county, state, and state abbreviation.

- Eviction state/county data was merged with location data using FIPS/GEOID codes. Social security and unemployment data were merged with the eviction data.  

![newplot](https://user-images.githubusercontent.com/54602329/65059429-ffd9a280-d943-11e9-8e93-231a6809c334.png)

 
To converge the compiled data with CoC homeless counts,I grouped the observations into CoC areas using the CoC number. Features containing totals were summed together. Features containing percentages, averages, or medians were averaged. Data was merged to create the initial dataset with 3157 observations and 34 columns. 

## Exploratory Data Analysis

A scatterplot graph of population size by homeless count showed outliers with homeless counts above 30,000 & population sizes above 10,000,000. Dropping CoCs: NYC, LA, Santa Barbara, Texas(balance), Houston.

![pop](https://user-images.githubusercontent.com/54602329/64926721-cfbcc300-d7ce-11e9-97c4-22c85755bf37.png)

I checked for missing data and found that 498 observations were missing from the eviction rate feature and missing 441 observations were missing from the eviction filing rate feature. NaN's were replaced with the mean for each feature. 

I visualized the distribution of the data finding that many features were not normal, violating one assumption of linear regression. 

![download](https://user-images.githubusercontent.com/54602329/65158868-2dd0ec80-da01-11e9-8d10-6d6f39c4361b.png)




## Feature Engineering

I created a new feature, mental health care spend, by taking per capita SMHA spend per state and multiplying by population per CoC area for years 2007-2013. I did not find data on per capita spend for years 2014-2016, this data was created by taking the average population size and per capita spend per observation. 

![mental](https://user-images.githubusercontent.com/54602329/65061125-44b30880-d947-11e9-9251-b95a9c650932.png)

## Transformations

After checking variable distrubtions, I applied a logarithmic function and observed the impact on each distribution.

![log](https://user-images.githubusercontent.com/54602329/65061897-da9b6300-d948-11e9-9282-a8549e939f2e.png)

Using Polynomial regression, variables were transformed twice into new interaction and polynomial features.

## Feature Selection

Features > .90 correlation were dropped, reducing the dataset from 434 features to 90 features. 

Lasso regularization reduced the features to 87. 

## Models 

Linear Regression, R2: 0.63, MAE: 0.49

Decision Tree Regression with Grid Search CV, R2: 0.83, MAE:0.26
 
 - Best parameters: max_depth= 18, min_samples_leaf= 5

Random Forest Regression with Grid Search CV, R2: 0.93, MAE: 0.19

 - Best parameters: max_depth= 18, n_estimators= 1000

XGBoost Regressor with Grid Search CV, R2: 0.75, MAE: 0.41

 - Best parameters: gamma= 1.0, learning_rate= 0.1, max_depth= 6, min_child_weight= 1, n_jobs= 4, subsample= 0.05

## Best Model: Random Forest Regression

_Top 10 Features:_

 - Total people 65 and older recieving Supplemental Security Income in the CoC
 
 - Population size
 
 - Population * Median Property Value
 
 - Percentage of Native Hawaiian and Pacific Islanders
 
 - Percentage of Renter Occupied Households * Percentage of White people
 
 - Eviction Rate *  Percentage of Hispanic people
 
 - Mental Health Budget
 
 - Percentage of Renter Occupied Households
 
 - Median Property Value
 
 - Poverty Rate * Eviction Filings

## Conclusion

A predictive model can potentially eliminate the problematic manual count and/or grant competitions, allowing for systematic distribution of resources. Funding for homelessness services should be determined by need, irrespective of the current political climate and free from institutional bias. Homelessness can be determined at the county level, which could lead to a community-based approach and stronger data collection. 

There are additional contributing factors to homelessness such as domestic violence, natural disasters, immigration policy, addiction, and divorce. Including some or all measurements of these factors could improve the model. 
