# Project_1
Relationships Between Health, GDP per Capita, and Trade

This project was undertaken to uncover the relationships between health indicators (life expectancy & infant mortality), productivity per capita, and trade indicators (exports & imports). It was assumed at the start that improved productivity would improve national well-being and that as nations got richer, they would export less (out of necessity) and import more.

A write-up was created to assign roles to group members, and then, data was pulled from https://data.worldbank.org. A Jupyter Notebook was created to perform the analysis and another was created to detail the process.


Data Exploration Process
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
We first tried to decide what type of data we wanted to look at, so we looked at several datasets on Kaggle.com.
We eventually decided on World_Development_Indicators.
This contained data with a value column and several rows for each combination of country, indicator type, and year.
-------------------------------------------------------------------------------------------------------------------
There were a couple of problems with this data.
One, the data only included data from years 1960 to 1980, so we couldn't make conclusions with contemporary data.
Two, in order to be able to do the things we wanted with the data, it would require transposing the rows and columns, which we weren't sure how to do, given the layout of the file.
Three, the merging created file sizes so large that it made it all but impossible to Git Push.
-------------------------------------------------------------------------------------------------------------------
For this reason, we pulled data from the World Bank website, which contains economic indicator data from years 1960 to 2019 for analysis.
This enabled us to work with the range of years we wanted and made merging far simpler, since we could merge everything by the CountryCode column.
Because we had all of the data we needed, we didn't need to pull data from any API.
We ended up chosing to look at: GDP per Capita, Life Expectancy, Infant Mortality, Exports, & Imports.
We started by attempting to look at the relationship between GDP per Capita and the remaining indicators.
We assumed that as nations became more productive, that they would experience improvements in well-being and increased imports.
However, we assumed that exports would go down, since nations that become more affluent would have more domestic purchasing power.
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 
Data Merging/Cleanup Process
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
We found that each type of indicator from the World Bank website had its own file with values in a year as separate columns and each country or area taking up only a single row.
There were a few columns that contained Country Name, Country Code, Indicator Name, and Indicator Code.
Every file had Country Code in common, so we decided to merge each file by that column.
We discovered that x & y are the only suffices that get added, so if three or more columns have the same name, there will nothing to differentiate them.
To get around this, we decided to use the Add_Suffix function to our dataframes to add the corresponding type of indictor name to the end of each column.
In order to be able to merge by the CountryCode column, we had to change the column name for that column to drop the suffix.
-------------------------------------------------------------------------------------------------------------------
With those considerations listed, the merging process was extremely simple.
Since every indicator type had the exact same rows and columns (not counting values), we simply had to merge by CountryCode and essentially append columns from one file to to end of the columns from another.
We first merged the GDP per Capita file with the Life Expectancy file to form summary_df.
After that, we merged the remaining files in a stepwise fashion into summary_df.
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------    
Data Analysis Process
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
We created scatter plots of GDP per Capita vs the trade and well-being indicators.
-------------------------------------------------------------------------------------------------------------------
In order to do the regression, we had to filter out NaN values, so we used numpy.isnan to create a mask that we applied in the columns used by the regression function.
-------------------------------------------------------------------------------------------------------------------
In each case, we started by creating plots for the years 1990 & 2017 in order to see how factors correlated in any given year.
We found that there were many outliers that skewed the data for individual years for the Exports vs GDP per Capita and Imports vs GDP per Capita plots for those years, so we decided to invesitgate further.
We then took the average yearly values across all countries and plot across all years.
We found that there was an extremely strong correlation between each trade indicator and GDP per Capita, we just needed to use the power of averaging to filter out the random noise variation.
-------------------------------------------------------------------------------------------------------------------
What we found validated our theory about imports, but was the reverse for exports.
-------------------------------------------------------------------------------------------------------------------
Before we proceded further, we graphed the yearly averages of Imports vs Exports and found that they had a near perfect 1:1 correlation.
From this, we assumed that this made the data more trustworthy.
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
We did the same thing for GDP per Capita vs the well-being indicators.
-------------------------------------------------------------------------------------------------------------------
This time around however, we found that there were clear trends for Life Expectancy vs GDP per Capita and Infant Mortality vs GDP per Capita.
However, the correlation was clearly non-linear, so we used logarithmic regression for life expectancy and power regression for infant mortality
To do those, we had to define a function and then use curve_fit instead of linregress.
Since curve_fit doesn't specify a rvalue, we had to manually calculate the rsquared value and then retroactively go back and then square the rvalue on previous plots for a direct comparison.
-------------------------------------------------------------------------------------------------------------------
We found that for individual years as well as across years, that the life expectancy increases & infant mortality decreases as GDP per Capita increases, but tapers off.
-------------------------------------------------------------------------------------------------------------------
From this, we concluded that trade makes countries more productive, which in turn, improves well-being of the citizenry.
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
To test our hypothesis, we decided to first look at the GDP per Capita values for the top 5 & bottom 5 countries for 1990 & 2017.
From this, we saw that there was an enormous gap of 2-3 orders of magnitude between the top 5 & bottom 5, which would explain differences in wealth.
After that, we compared the trade & well-being values against GDP per Capita for 1990 and 2017 for countries in the bottom 5 for either year.
Essentially, we wanted to see whether an uptick in trade would predict a country "escaping" the bottom 5 and whether that would lead to an increase in well-being.
-------------------------------------------------------------------------------------------------------------------
For trade, we found that: of the countries of Nepal, Tanzania, and Sierra Leone that “escaped” the bottom 5, Nepal & Tanzania had the largest increases in trade, but Sierra Leone didn’t.
This would likely indicate that trade can improve the productivity of a country, but it is not a perfect predictor for individual countries between two separate years.
------------------------------------------------------------------------------------------------------------------
For well-being, we found that: of the countries of Nepal, Tanzania, and Sierra Leone that “escaped” the bottom 5, all had relatively large improvements in life expectancy & infant mortality rates, but not always the largest.
This would likely indicate that productivity of a country can improve well-being, but it is not a perfect predictor for individual countries between two separate years.
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
Conclusions
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. Exports & Imports have a strong correlation with GDP per Capita.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2. Life Expectancy & Infant Mortality have a strong correlation with GDP per Capita.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
3. Exports & Imports have a perfect correlation when averaging across all countries.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
4. The nations with the bottom 5 lowest GDP per capita values from 1990 & 2017 were compared, and those that had the lowest in 1990 were no longer in the bottom 5 when they had a big uptick in trade or high trade to start with.
-These were Nepal & Tanzania.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
5. These nations also saw an increase in life expectancy & sharp decreases in infant mortality.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
6. The way to lift poor countries out of poverty and increase their well-being is to increase trade with them.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
7. Poor countries should find ways to leverage their comparative advantages in order to attract trade partners.
