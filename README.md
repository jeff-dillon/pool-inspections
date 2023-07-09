# Pool Inspections
Example Code Louisville Data Analysis Project. Spreadsheet analysis of pool inspection data.

## Demo Overview

The purpose of this demo is to show the data analysis process and concepts using a spreadsheet before jumping into the details of coding in Python. This demo is broken down into three steps.

At a high level, this demo will show:
1. How to **describe a data set** including the shape of the data, the types of data in the columns, and what the data in the columns looks like. The purpose of this step is to understand what cleaning will be needed and what questions can be answered.
1. How to **clean a data set** including dealing with missing records and creating new columns based on existing ones.
1. How to **analyze and visualize a data set** by grouping and aggregating data and visualizing it in tables and graphs.  


## Demo Instructions

1. Fork the repo and clone your forked repo to your machine.
1. Download the [Louisville Metro KY - Inspection Results Pools](https://data.louisvilleky.gov/datasets/LOJIC::louisville-metro-ky-inspection-results-pools/about) 
data and save it as a spreadsheet (Either Excel or Google Sheets will work).
1. When you have completed the exercise, either add your Excel to the repo and commit/push it up to GitHub or add a link 
to your Google sheet to the README and commit/push it up to GitHub.
1. Share your repo with your mentors if you would like feedback.

## Demo Setup

It is a good idea to have a clean, simple structure for your spreadsheet to make it easier for others to understand your
analysis.

1. Open the downloaded spreadsheet and change the name of the tab with the data in it to `00_Source`.
1. Create the rest of the tabs:

| Tab | Description |
| ----- | ----- |
| `00_Source` | Source data downloaded from Louisville Metro Open Data. Data in this tab will not be modified. |
| `01_Info` | We will use this tab to document the dataset. |
| `02_Calc` | We will use this tab to create calculated fields based on existing fields using formulas. |
| `03_Clean` | We will use this tab to store the final clean data for use in analysis. |
| `04_Analysis` | We will use this tab to show pivot tables and charts that we will use to answer questions about the dataset. |

## Demo 1: Data Discovery

The first step in the data analysis process is to describe the data in the dataset. This will help you understand the
source data and inform what data preparation will be needed and what questions you will be able to answer with this 
dataset.

### Instructions

Document the following in the `01_Info` tab:

1. Provide a link to the data source and a description of the data
1. Describe the shape of the data (# rows and # cols)
1. Define the columns / fields in the data using a data dictionary (a table containing the list of columns with the 
column name, data type, description, and cleaning notes).
1. Summarize the # of records for each distinct value in the categorical columns.
1. Summarize the descriptive statistics (min, max, mean, median, mode, std. dev.) for each numerical column. 
1. Document the cleaning needed including what to do with outlier records, what columns will not be needed, etc.
1. Document the questions you will be able to answer with the data.
1. Document any new columns that will need to be created based on calculations using existing columns.


### Notes: 

- You can read more about types of data [here](https://www.pluralsight.com/guides/data-literacy-essentials:-representing-processing-and-preparing-data#module-typesofdata).
- Pivot tables are an easy way to summarize the distinct values in columns. For Example, a pivot table could list the distinct cities in the data with a count of the number of records for each city. You can read more about pivot tables [here](https://support.microsoft.com/en-gb/office/overview-of-pivottables-and-pivotcharts-527c8fa3-02c0-445a-a2db-7794676bce96).
- You can read about calculating descriptive statistics in a spreadsheet using functions [here](https://www.statology.org/descriptive-statistics-google-sheets/). 
- You can read about the types of issues that need to be addressed when cleaning data [here](https://www.pluralsight.com/guides/data-literacy-essentials:-representing-processing-and-preparing-data#module-preparingdata).
- Some example questions you could ask of this data include: In which year were most facilities opened? Do facilities 
targeted at children have higher or lower scores than those targeted at all age groups? Which facilities have improved 
their scores?

        
## Demo 2: Data Cleaning

The next step in the data analysis process is to clean the data in preparation for analysis. This includes removing invalid records, creating new calculated fields, and renaming columns if needed.

### Instructions

Add the following columns to the `02_Calc` tab:

1. Copy the `ObjectId` column from `01_Source`. We'll use this to "look up" or "join" values from the source data. 
2. Add a column called `opening_year` and calculate the opening year for each record in the source data as the year portion of the opening_date column. Example: "8/9/2022 0:00:00" should become "2022".
3. Add a column called `target_audience` and calculate the target audience based on the subtype for each record in the source data. "SPLASH PAD" and "WADING POOL" should be "Kid". All Others should be "General".
4. Add a column called `score_trend` and calculate the trend for each record in the source data. If `ScoreREcent` > `Score3` the trend is "Increasing". If `ScoreRecent` < `Score3` the trend is "Decreasing". Otherwise, the trend is "Same".

Combine the Source Data and Calculated Fields in the `03_Clean` tab:

1. Copy all of the data from the Raw Data. 
1. Remove any rows as defined in the cleaning notes.
1. Remove any unneeded columns as defined in the cleaning notes.
1. "Lookup" the calculated columns from `02_Calc`.

### Notes

- You can read about lookups in Google Sheets [here](https://spreadsheetpoint.com/how-to-use-lookup-in-google-sheets/)
or in Excel [here](https://support.microsoft.com/en-au/office/xlookup-function-b7fd680e-6d10-43e6-84f9-88eae8bf5929).

### Formulas

Here are example Excel/GoogleSheets formulas to calculate the new columns.

| field | formula |
| -----  | ----- |
| target_audience | `=if(or(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!K:K) = "SPLASH PAD",XLOOKUP(A2,'01_Source'!R:R,'01_Source'!K:K) = "WADING POOL"), "Kids", "General")` |
| score_trend | `=if(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!A:A) > XLOOKUP(A2, '01_Source'!R:R,'01_Source'!E:E),"Increase",if(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!A:A) < XLOOKUP(A2, '01_Source'!R:R,'01_Source'!E:E),"Decrease","Same"))` |
| opening_year | `=IF(YEAR(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!Q:Q))=1899,"Unknown",YEAR(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!Q:Q)))` |



## Demo 3: Data Analysis & Visualization

The last step is to analyze and visualize the data to answer the questions identified in the Data Discovery step.

Add the following anlysis to the `04_Analysis` tab:

1. A pivot table and graph summarizing the number of facilities by year.
1. A pivot table and graph summarizing the average score by facility target audience.
1. A pivot table and graph summarizing the number of facilities by score trend.
