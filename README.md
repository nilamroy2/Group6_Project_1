# Medicare-for-Py

Healthcare is one of the top issues up for debate in the 2020 presidential election. The idea of “Medicare-for-all” would bring the US closer to how the rest of the world provides health care to their residents…which has arguably shown to be successful.


-Define the core message or hypothesis of your project.
-Describe the questions you asked, and why you asked them
-Describe whether you were able to answer these questions to your satisfaction, and briefly summarize your findings

### Questions & Data
We wanted to analyze the current landscape of Medicare in the United States and answer the following questions:

* How have the hospital charges for services for Medicare beneficiaries changed over time?
* Is there a difference in what each state charges Medicare beneficiaries to treat the same diagnosis?
* How much is not getting covered by Medicare?
* What diagnosis are Medicare beneficiaries being seen for the most? Least?


To attempt to find answers to the questions we had we used data from the Centers for Medicare & Medicaid Services

CMS is a part of the Dept of Health and Human Services. 

Flat files were gathered from https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/medicare-provider-charge-data/inpatient.html


### Data Cleanup & Exploration

* From cms.gov we found several flat files with Medicare provider information from 2011-2016

* Data from more recent years would have been preferable, but considering the limitations of government datasets, we decided that 2016 would be sufficient

* Soon after working with the data we uncovered that the available information did not match from year to year. 2014-2016 had significantly more information than previous years

* Multiple columns in the file were saved as strings. Needed to remove special characters and ensure the values were being read as floats

```
# covert values from string to int/float so that we can use it for calculations
# remove characters "$" and commas ","

state_df[" Total Discharges "] = state_df[" Total Discharges "].str.replace("," , "")
state_df["Average Covered Charges"] = state_df["Average Covered Charges"].str.replace("," , "").str.replace("$" , "")
state_df["Average Total Payments"] = state_df["Average Total Payments"].str.replace("," , "").str.replace("$" , "")
state_df["Average Medicare Payments"] = state_df["Average Medicare Payments"].str.replace("," , "").str.replace("$" , "")

# with characters removed, covert to float
state_df["Average Covered Charges"] = pd.to_numeric(state_df["Average Covered Charges"])
state_df["Average Total Payments"] = pd.to_numeric(state_df["Average Total Payments"])
state_df["Average Medicare Payments"] = pd.to_numeric(state_df["Average Medicare Payments"])
state_df[" Total Discharges "] = pd.to_numeric(state_df[" Total Discharges "])

#state_df.head()
```

### Data Analysis



### Barriers and other questions

### Final Observations
