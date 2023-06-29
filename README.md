# Pool Inspections
Example Code Louisville Data Analysis Project. Spreadsheet analysis of pool inspection data.

## Setup

1. Clone the repo to your machine.
2. Download the [Louisville Metro KY - Inspection Results Pools](https://data.louisvilleky.gov/datasets/LOJIC::louisville-metro-ky-inspection-results-pools/about) data and save as a spreadsheet (Either Excel or Google Sheets will work).
3. Change the name of the tab with the data in it to "01_Source".

## Data Discovery

The first step is to describe the data in the dataset. This will inform what data preparation will be needed and what questions you will be able to answer with this dataset.

1. Add a `00_Info` tab to the spreadsheet. We will use this tab to document the dataset. Add a link to the Data Source, a Summary Description of the dataset, and the Data Shape (# cols and rows) to the `00_Info` tab.
2. Add a Data Dictionary (a list of columns) to the `00_Info` tab including the Name, Type, Description, and Cleaning Notes for each column.
3. Create a pivot table and chart of the count of records for each distinct value for the following columns on the `00_Info` tab: Zip, City, Subtype, Score. For Example, the City pivot should list the distinct cities in the data with a count of the number of records for each city.
4. Document the Cleaning Needed in the `00_Info` tab. What records need to be removed from the raw data?
5. Document the Questions to Answer in the `00_Info` tab. What questions can you answer given the available data?
6. Document the Features to Create in the `00_Info` tab. What calculated columns will you have to create in order to answer the questions? (opening_year, target_audience, score_trend)

        
## Data Cleaning

The next step is to prepare the data for analysis. This includes removing invalid records, creating new calulated fields, and renaming columns if needed.

1. Add a `02_Calc` tab to the spreadsheet. Copy the ObjectId column from 01_Source into this tab.
2. Add a column to `02_Calc` called `opening_year` and calculate the opening year for each record in the source data as the year portion of the opening_date column. Example: "8/9/2022 0:00:00" should become "2022".
3. Add a column to `02_Calc` called `target_audience` and calculate the target audience based on the subtype for each record in the source data. "SPLASH PAD" and "WADING POOL" should be "Kid". All Otheres should be "General".
4. Add a column to `02_Calc` called score_trend and calculate the trend for each record in the source data. If `ScoreREcent` > `Score3` the trend is "Increasing". If `ScoreRecent` < `Score3` the trend is "Decreasing". Otherwise the trend is "Same".
5. Add a `03_Clean` tab. Copy the data from the Raw Data. Remove the rows as defined in the cleaning. Vlookup the additional columns from `02_Calc`.

### Formulas

Here are example Excel/GoogleSheets formulas to calculate the new columns.

| field | formula |
| -----  | ----- |
| target_audience | `=if(or(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!K:K) = "SPLASH PAD",XLOOKUP(A2,'01_Source'!R:R,'01_Source'!K:K) = "WADING POOL"), "Kids", "General")` |
| score_trend | `=if(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!A:A) > XLOOKUP(A2, '01_Source'!R:R,'01_Source'!E:E),"Increase",if(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!A:A) < XLOOKUP(A2, '01_Source'!R:R,'01_Source'!E:E),"Decrease","Same"))` |
| opening_year | `=IF(YEAR(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!Q:Q))=1899,"Unknown",YEAR(XLOOKUP(A2,'01_Source'!R:R,'01_Source'!Q:Q)))` |



## Data Analysis & Visualization

The last step is to analyze and visualize the data to answer the questions.

1. Add a `04_Analysis` tab with a pivot table and chart answering each of the questions defined in `00_Discovery`.
