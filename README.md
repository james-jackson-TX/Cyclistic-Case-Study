![This is an image](https://bayodeolorundare.com/wp-content/uploads/2023/02/tableau_hero.jpg)
# Cyclistic Case Study
*Google Data Analyst Capstone Project*

For this project, I am a junior data analyst working in the marketing analyst team at Cyclistic, a fictional bike-share company in Chicago. I am tasked with understanding how casual riders and annual members use Cyclistic bikes differently.  While members subscribe annually for unlimited access to bikes, casual riders purchase single ride or full day passes.  Member are typically locals who use the bikes for commutes to work daily among other things.  While casual riders include many tourists (non-locals), many locals also use the service.  From teh insights gained in this analysis, the team will design a marketing strategy to convert casual riders into annual members.

In order to answer the business questions, I will follow the steps of the data analysis process: Ask, Prepare, Process, Analyze, Share, and Act.

# 1 ASK
## 1.1 Ask the right questions
Three questions will guide the future marketing program:
 1. How do annual members and casual riders use Cyclistic bikes differently?
 2. Why would casual riders buy Cyclistic annual memberships?
 3. How can Cyclistic use digital media to influence casual riders to become members?

I have been tasked to answer the first question "How do annual members and casual riders use Cyclistic bikes differently?". 

## 1.2 Key Stakeholders
1. Lily Moreno, Marketing Director: She is responsible for the development of campaigns and initiatives to promote the program. She uses the data collected and inferred by the marketing analytics team to make relevant marketing strategies for the program.
2. Executive Team: They are responsible for reviewing the recommendations and approoving the marketing strategy.
3. Marketing Analytics Team: They are responsible for collecting, analyzing, and reporting data to help guide marketing strategy.

## 1.3 Identify the business task
This analysis analyzed 12 months of historical bike trip data to identify the differences between member rider usage and casual rider usage.  

# 2 PREPARE
This step includes identifying and collecting the data from its location; determining its integrity, credibility and accessibility; and storing the data for analysis.

## 2.1 Data Location
For this analysis, I will be using Cyclistic historical bike trip data, which is publicly available [here](https://divvy-tripdata.s3.amazonaws.com/).  

## 2.2 Data Organization
The data is stored in CSV files for each month.  As I will be using data for Jan 2023 - Dec 2023, there are 12 CSV files toreview and download.
```R
2023-01_divvy_trip_data.csv
2023-02_divvy_trip_data.csv
2023-03_divvy_trip_data.csv
2023-04_divvy_trip_data.csv
2023-05_divvy_trip_data.csv
2023-06_divvy_trip_data.csv
2023-07_divvy_trip_data.csv
2023-08_divvy_trip_data.csv
2023-09_divvy_trip_data.csv
2023-10_divvy_trip_data.csv
2023-11_divvy_trip_data.csv
2023-12_divvy_trip_data.csv
```

The data is structured, organized in columns and rows, with each row representing an individual ride represented by a unique ride_id.  Trip data is anonymized and includes no personally identifiable information.  Each file contains 13 columns, all consistently named as follows:
```R
* ride_id               #Ride id - unique
* rideable_type         #Bike type - Classic, Docked, Electric
* started_at            #Trip start day and time
* ended_at              #Trip end day and time
* start_station_name    #Trip start station
* start_station_id      #Trip start station id
* end_station_name      #Trip end station
* end_station_id        #Trip end station id
* start_lat             #Trip start latitude  
* start_lng             #Trip start longitute   
* end_lat               #Trip end latitude  
* end_lat               #Trip end longitute   
* member_casual         #Rider type - Member or Casual 
```

## 2.3 Data Credibility
Reviewing the five characteristics of data, i have determined that the data is credible and free from bias.
1. Reliable: Datasets are accurate, complete and unbiased information that's been vetted and proven fit for use.
2. Original: The data is coming from the source, not third parties.
3. Comprehensive: The data contain the critical information needed to answer the question.
4. Current: The data is the most recent year (2023).
5. Cited: Vetted publicly available data used in many various analyses projects.

## 2.4 Data License
The data is made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement).

## 2.5 Data Limitations
THe data does not contain information that would assist in detemrining if the riders are locals or visitors.  The data does not contain information for casual riders on which type of pass was purchased, single ride or full day.  The data does not contain any user information so it is impossible to know how many trips were one time rides or from repeat riders.  The data can tell me how many rides, but will not provide insight on number of customers or frequency of rides for an individual rider.

## 2.6 Download data
