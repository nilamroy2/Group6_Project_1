# Medicare-for-Py

Healthcare is one of the top issues up for debate in the 2020 presidential election. The idea of “Medicare-for-all” would bring the US closer to how the rest of the world provides health care to their residents…which has arguably shown to be successful.


-Define the core message or hypothesis of your project.
-Describe the questions you asked, and why you asked them
-Describe whether you were able to answer these questions to your satisfaction, and briefly summarize your findings

## Questions & Data
We wanted to analyze the current landscape of Medicare in the United States and answer the following questions:

* How have the hospital charges for services for Medicare beneficiaries changed over time?
* Is there a difference in what each state charges Medicare beneficiaries to treat the same diagnosis?
* How much is not getting covered by Medicare?
* What diagnosis are Medicare beneficiaries being seen for the most? Least?


To attempt to find answers to the questions we had we used data from the Centers for Medicare & Medicaid Services

CMS is a part of the Dept of Health and Human Services. 

Flat files were gathered from https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/medicare-provider-charge-data/inpatient.html


## Data Cleanup & Exploration

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

## Data Analysis
##### How have Medicare provider charges changed overtime?
* When comparing all available diagnosis, we see that California, Florida, Texas, New York and Pennsylvania charge the most for Medicare covered services. 

* However, this trend seems to  correlate with states that have higher populations

![image](https://user-images.githubusercontent.com/49836101/60748780-16807800-9f57-11e9-962e-9eec3dd949c0.png)

* When viewing average charges per provider, versus the whole state we see some variation in which states are charging Medicare beneficiaries more.

![image](https://user-images.githubusercontent.com/49836101/60748796-36b03700-9f57-11e9-9547-505bd04253a1.png)

* In both charts we see a slight increase in charges from 2014 - 2016

#### Is there a difference in what each state charges Medicare beneficiaries to treat the same diagnosis?
![image](https://user-images.githubusercontent.com/49836101/60748617-59415080-9f55-11e9-8b84-9788e7c86850.png)

* When comparing 5 diagnosis and 5 select states we see the following trend:
Providers in California, Florida and Texas charge similar rates for heart and renal failure related diagnosis

* Overall, states are charging different rates to treat the same diagnosis

**Medicare Provider Charges in Texas**

![image](https://user-images.githubusercontent.com/49836101/60748619-5cd4d780-9f55-11e9-8b78-0d3fe84d256b.png)

* We found that not only do provider charges vary across states, but there are also differences in charges for the same diagnosis within the same state
* While there is some regulation, each individual provider/hospital chooses what they want to charge patients. 

![image](https://user-images.githubusercontent.com/49836101/60748622-62322200-9f55-11e9-82e8-137c66321da2.png)

* In Texas, Methodist, Houston Methodist and Baylor Medical Center  are able to treat the most diagnosis for Medicare beneficiaries

#### How much is not getting covered by Medicare?

**After Medicare makes a payment to the provider, how much is left to be covered by the patient or other entities?**

* For select diagnosis, we can determine that Alaska offers fair Medicare plans to its residents, since only 10% of the total payment remains after Medicare pays its part. 

![image](https://user-images.githubusercontent.com/49836101/60748959-d91cea00-9f58-11e9-9887-2f4fda29e811.png)

**When comparing the same 5 states and diagnosis as previously mentioned, we observed the following:**
* There would still be more than 40% of the total balance remaining after Medicare pays its part in both Florida and North Dakota.
* On a previous graph we saw that Vermont had one of the higher overall Medicare service charges, but conversely, Medicare plans in this state would cover ~80% of the cost.

![image](https://user-images.githubusercontent.com/49836101/60748947-b5f23a80-9f58-11e9-9976-55530ed4fb5e.png)

#### What diagnosis is being treated the most among Medicare beneficiaries?

![image](https://user-images.githubusercontent.com/49836101/60748632-6e1de400-9f55-11e9-9379-5de30847a720.png)

* Diagnosis of sepsis, joint replacement and heart failure are most common among patients using Medicare as a payer. 
* Less common diagnosis included multiple labor and delivery diagnosis. 
* Considering that eligible Medicare recipients include older adults (65+) and disabled individuals these observations were expected.

* Less expected was the difference in what Medicare would cover for the same diagnosis across different states.

* It is clear that more populated states have a higher number of Medicare discharges, but we also see that the Medicare plans in some of these same states cover just a small fraction of the total charges

* While Florida historically has one the higher retirement populations and hence higher Medicare beneficiary discharges, we see that Massachusetts is a leader when it come to health care reform

![image](https://user-images.githubusercontent.com/49836101/60748635-724a0180-9f55-11e9-9ce0-27c868905cb0.png)


## Barriers 
__Data for years 2011-2013 had significantly less diagnosis information than 2014-2016.__
* To address this we focused on the datasets for 2014 – 2016 and some charts focus on specific diagnosis that we know are treated throughout all 50 states. 
__Our team was unknowingly referencing different data sources__
* The site where we pulled our data from had multiple links with very similar, yet different! files. We later discovered that we were all not using the same data and had to go back and make corrections. 
__Formatting the dataset__
* Significant time was spent formatting columns into floats and finding single errors to correct. Some of the csv files had inconsistent formatting while other did not.
* For example, discovering that one column name was formatted differently that the others. The “Total Discharges” column was formatted with a space at the beginning and end. Attempting to call this column without discovering this just resulted in errors

## Other Questions

__What amount are the patients responsible for paying?__
* The data provided information about how much Medicare would pay, what the provider was charging and what the total payment was, but we would need more information to say exactly how much the patient was responsible for
__What are states like Massachusetts doing differently than the others?__
* When considering the top discharged diagnosis, Massachusetts was an outlier in how much Medicare would cover for this service. In MA Medicare would cover more than 45% of the cost for treatment of severe sepsis. The next highest state covering less than 35%
__How do public, private and faith-based hospitals compare in cost?__
* It is clear that the cost of Medicare services will vary depending on the provider, but are there any trends in the type of hospital that is offering the service?

* If we had more time, perhaps we could examine data by smaller regions rather than states (zip codes) and draw on other CSVs or APIs to determine factors about those zip codes and find correlations with charges from hospital providers for Medicare patients.

## Final Observations

* Even though Medicare is managed on a Federal level, coverage varies significantly depending on both the state and the provider/hospital themselves. 

* On average, 15 – 20% of the total payment still remains after Medicare submits its payment to the provider. The patient will be responsible for an unknown portion of this. 

* The most common diagnosis among Medicare beneficiaries are related to sepsis (infection), joint replacement, heart failure and renal failure. 

* The cost of Medicare has certainly increased over the years along with other types of health insurance, but more data would be needed to explicitly say by how much.

* Lastly…healthcare in the US has room for improvement! 
