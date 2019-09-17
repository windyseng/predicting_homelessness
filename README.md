# Predicting Homeless Population Sizes 

The United States Department of Housing and Urban Development(HUD)award grants to Continuum of Care applicants through an annual competition. A Continuum of Care (CoC) is a regional or local planning body that coordinates housing and services funding for homeless families and individuals. CoC's distribute the funding to nonprofits and local government programs across the regional areas they represent. The population size and geographic areas covered by CoCs are dramatically different across state lines. 

Two critical activities entrusted to CoCs is the biannual physical count of homeless people and an annual enumeration of transitional housing units and homeless shelter beds that are being used. These counts provide an overview of the state of homelessness in a CoC area and help determine funding allocation. 

In this project, I will show that through machine learning and using publicly available data; we can predict homeless population sizes, thereby eliminating the problematic manual counting and/or grant competitions, allowing for systematic distribution of baseline funding. 

## Primary Data Sources | Years 2007:2016

[US Department of Housing and Urban Development: CoC Homeless Counts](https://www.hudexchange.info/resource/3031/pit-and-hic-data-since-2007/)

[US Social Security Administration: Supplemental Security Income Annual Reports](https://www.ssa.gov/policy/docs/statcomps/ssi_sc/2016/index.html) 

[US Department of Labor's Bureau of Labor Statistics: Unemployment Rates](https://www.kaggle.com/jayrav13/unemployment-by-county-us/data)

[Princeton University: Eviction Lab Statistics](https://data-downloads.evictionlab.org/)

[Kaiser Family Foundation: State Mental Health Agency Expenditures per Capita](https://www.kff.org/other/state-indicator/smha-expenditures-per-capita/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D)


[HUD Exchange: Continuum of Care Coverage](https://www.hudexchange.info/resource/4981/fy-2016-continuums-of-care-names-and-numbers/)
*(compiled the county list manually)*

## Compiling Data 

- Annual HUD CoC files were converted into a single dataframe containing CoC number, year, homeless count. 

- Excel workbooks of annual Social Security data contained sheets for each state with spend itemized by county and demographic on each sheet. The data from 50 pages of each workbook were combined into dataframes, reformatted, and the year was added. The annual data was concatenated into one dataframe. 

- Unemployment data was already compiled via Kaggle, state and county data was reformatted and the monthly items were grouped into annual averages. 

- Location data was compiled into a dataframe with CoC number, FIPS code, latitude, longitude, county, state, and state abbreviation.

- Eviction state/county data was merged with location data using FIPS/GEOID codes. Social security and unemployment data was merged with the eviction data.  

![newplot](https://user-images.githubusercontent.com/54602329/65059429-ffd9a280-d943-11e9-8e93-231a6809c334.png)

 
To converge with CoC homeless counts, the data was grouped into CoC areas using the CoC number. Features containing totals were summed together. Features containing percentages, averages, or medians were averaged. Data was merged to create the initial dataset with 3157 observations and 34 columns. 

## EDA

A line graph the homeless count by CoC showed two large outliers. 

![count](https://user-images.githubusercontent.com/54602329/64926615-98014b80-d7cd-11e9-90f3-39ffe3ae3f46.png)

A scatterplot graph of population size by homeless count showed additional outliers with homeless counts above 30,000 & population sizes above 10,000,000. Dropping CoCs: NYC, LA, Santa Barbara, Texas(balance), Houston.

![pop](https://user-images.githubusercontent.com/54602329/64926721-cfbcc300-d7ce-11e9-97c4-22c85755bf37.png)

Eviction rate was missing 498 observations and eviction filing rate was missing 441 observations. NaN's were replaced with the mean for each feature. 


Initial look at distributions across key features. 

![dist](https://user-images.githubusercontent.com/54602329/65059832-ae7de300-d944-11e9-986a-8e705251eccb.png)

## Feature Engineering

New feature: mental healthcare spend. Taking per capita spend per state and multiplying by population per CoC area for years 2007-2013. I did not find data on per capita spend for years 2014-2016, this data was created by taking the average population size and per capita spend per observation. 



