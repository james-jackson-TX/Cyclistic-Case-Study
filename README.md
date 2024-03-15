![This is an image](https://i.imgur.com/vfJkCrn.png)
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
For this analysis, I will be using Cyclistic historical bike trip data, which is publicly available [here](https://divvy-tripdata.s3.amazonaws.com/index.html).  

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
The data does not contain information that would assist in detemrining if the riders are locals or visitors.  The data does not contain information for casual riders on which type of pass was purchased, single ride or full day.  The data does not contain any user information so it is impossible to know how many trips were one time rides or from repeat riders.  The data can tell me how many rides, but will not provide insight on number of customers or frequency of rides for an individual rider.

## 2.6 Download data
Data was downloaded and named consistently.

## 2.7 Check for duplicates and blanks
I opened each of the 12 monthly files in Excel and checked for duplicate records - no duplicate ride_id information
Then I checked for blank fields.  There were quite a lot of blanks (over 800K), but they were all in Start/End Station Name/ID columns and Start/End Lat/Long columns.  Due to the possibility of skewing results if those records were removed, I opted to fill the blanks with "Not Available" and keep the records.

## 2.8 Create Data Frame using R Studio
To better manage the task, the 12 monthly files need to be combined into one file which would result in a file with over 5 million rows. With data that large, Excel is not the right tool for further analysis.  I opted to use R Studio for the remainder of my analysis work.

I started by opening R Studio and installing the packages I would need for this project.
```R
## Install needed packages
install.packages("tidyverse")
install.packages("lubridate")
install.packages("ggplot2")
install.packages("skimr")

library(tidyverse)
library(readr)
library(dplyr)
library(lubridate)
library(magrittr)
library(ggplot2)
library(skimr)
```
After uploading the files to R Studio I created the 12 data frames and then looked at the column names for each file confirming that each file contained the same exact column names.
```R
> colnames(X202301)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202302)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202303)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202304)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202305)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202306)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202307)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202308)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202309)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202310)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202311)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> colnames(X202312)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual" 
```
Next, I will inspect the data frames to ensure the data in the columns is in the same format across all 12 sheets
```R
> glimpse(X202301)
Rows: 190,301
Columns: 13
$ ride_id            <chr> "F96D5A74A3E41399", "13CB7EB698CEDB88", "BD88A2E670661CE5", "C90792D034FED968", "3397017529188E8A", "58E68156DAE3E311", "2F7…
$ rideable_type      <chr> "electric_bike", "classic_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "electric_bike", "classic…
$ started_at         <chr> "1/21/2023 20:05", "1/10/2023 15:37", "1/2/2023 7:51", "1/22/2023 10:52", "1/12/2023 13:58", "1/31/2023 7:18", "1/15/2023 21…
$ ended_at           <chr> "1/21/2023 20:16", "1/10/2023 15:46", "1/2/2023 8:05", "1/22/2023 11:01", "1/12/2023 14:13", "1/31/2023 7:21", "1/15/2023 21…
$ start_station_name <chr> "Lincoln Ave & Fullerton Ave", "Kimbark Ave & 53rd St", "Western Ave & Lunt Ave", "Kimbark Ave & 53rd St", "Kimbark Ave & 53…
$ start_station_id   <chr> "TA1309000058", "TA1309000037", "RP-005", "TA1309000037", "TA1309000037", "TA1309000019", "TA1309000037", "TA1309000037", "T…
$ end_station_name   <chr> "Hampden Ct & Diversey Ave", "Greenwood Ave & 47th St", "Valli Produce - Evanston Plaza", "Greenwood Ave & 47th St", "Greenw…
$ end_station_id     <chr> "202480", "TA1308000002", "599", "TA1308000002", "TA1308000002", "202480", "TA1308000002", "TA1308000002", "TA1308000002", "…
$ start_lat          <dbl> 41.92407, 41.79957, 42.00857, 41.79957, 41.79957, 41.92607, 41.79955, 41.79957, 41.79959, 41.79957, 41.79957, 41.79957, 41.9…
$ start_lng          <dbl> -87.64628, -87.59475, -87.69048, -87.59475, -87.59475, -87.63886, -87.59462, -87.59475, -87.59467, -87.59475, -87.59475, -87…
$ end_lat            <dbl> 41.93000, 41.80983, 42.03974, 41.80983, 41.80983, 41.93000, 41.80983, 41.80983, 41.80983, 41.80983, 41.80983, 41.80983, 41.9…
$ end_lng            <dbl> -87.64000, -87.59938, -87.69941, -87.59938, -87.59938, -87.64000, -87.59938, -87.59938, -87.59938, -87.59938, -87.59938, -87…
$ member_casual      <chr> "member", "member", "casual", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202302)
Rows: 190,445
Columns: 13
$ ride_id            <chr> "CBCD0D7777F0E45F", "F3EC5FCE5FF39DE9", "E54C1F27FA9354FF", "3D561E04F739CC45", "0CB4B4D53B2DBE05", "C67EB62172C472EB", "08A…
$ rideable_type      <chr> "classic_bike", "electric_bike", "classic_bike", "electric_bike", "electric_bike", "classic_bike", "classic_bike", "classic_…
$ started_at         <chr> "2/14/2023 11:59", "2/15/2023 13:53", "2/19/2023 11:10", "2/26/2023 16:12", "2/20/2023 11:55", "2/24/2023 18:50", "2/28/2023…
$ ended_at           <chr> "2/14/2023 12:13", "2/15/2023 13:59", "2/19/2023 11:35", "2/26/2023 16:39", "2/20/2023 12:05", "2/24/2023 18:56", "2/28/2023…
$ start_station_name <chr> "Southport Ave & Clybourn Ave", "Clarendon Ave & Gordon Ter", "Southport Ave & Clybourn Ave", "Southport Ave & Clybourn Ave"…
$ start_station_id   <chr> "TA1309000030", "13379", "TA1309000030", "TA1309000030", "TA1307000160", "TA1308000050", "TA1308000050", "TA1308000050", "TA…
$ end_station_name   <chr> "Clark St & Schiller St", "Sheridan Rd & Lawrence Ave", "Aberdeen St & Monroe St", "Franklin St & Adams St (Temp)", "Cottage…
$ end_station_id     <chr> "TA1309000024", "TA1309000041", "13156", "TA1309000008", "KA1503000054", "TA1307000115", "TA1307000115", "TA1307000115", "TA…
$ start_lat          <dbl> 41.92077, 41.95788, 41.92077, 41.92087, 41.79483, 41.91213, 41.91213, 41.91213, 41.91213, 41.91213, 41.89102, 41.91213, 41.9…
$ start_lng          <dbl> -87.66371, -87.64958, -87.66371, -87.66373, -87.61879, -87.63466, -87.63466, -87.63466, -87.63466, -87.63466, -87.63548, -87…
$ end_lat            <dbl> 41.90799, 41.96952, 41.88042, 41.87943, 41.78053, 41.90461, 41.90461, 41.90461, 41.90461, 41.90461, 41.90461, 41.90461, 41.9…
$ end_lng            <dbl> -87.63150, -87.65469, -87.65552, -87.63550, -87.60597, -87.64055, -87.64055, -87.64055, -87.64055, -87.64055, -87.64055, -87…
$ member_casual      <chr> "casual", "casual", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202303)
Rows: 258,678
Columns: 13
$ ride_id            <chr> "6842AA605EE9FBB3", "F984267A75B99A8C", "FF7CF57CFE026D02", "6B61B916032CB6D6", "E55E61A5F1260040", "123AAD676850F53C", "592…
$ rideable_type      <chr> "electric_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "classic_bike", "classic_bike", "docked_b…
$ started_at         <chr> "3/16/2023 8:20", "3/4/2023 14:07", "3/31/2023 12:28", "3/22/2023 14:09", "3/9/2023 7:15", "3/22/2023 17:47", "3/8/2023 19:5…
$ ended_at           <chr> "3/16/2023 8:22", "3/4/2023 14:15", "3/31/2023 12:38", "3/22/2023 14:24", "3/9/2023 7:26", "3/22/2023 18:01", "3/8/2023 20:0…
$ start_station_name <chr> "Clark St & Armitage Ave", "Public Rack - Kedzie Ave & Argyle St", "Orleans St & Chestnut St (NEXT Apts)", "Desplaines St & …
$ start_station_id   <chr> "13146", "491", "620", "TA1306000003", "18067", "620", "KA1503000044", "13431", "13074", "TA1308000029", "TA1307000136", "TA…
$ end_station_name   <chr> "Larrabee St & Webster Ave", "Not Available", "Clark St & Randolph St", "Sheffield Ave & Kingsbury St", "Sangamon St & Lake …
$ end_station_id     <chr> "13193", "Not Available", "TA1305000030", "13154", "TA1306000015", "TA1309000061", "TA1306000012", "TA1305000010", "TA130700…
$ start_lat          <dbl> 41.91841, 41.97000, 41.89820, 41.88872, 41.91448, 41.89820, 41.89017, 41.86610, 41.96522, 41.88683, 41.93650, 41.88848, 41.9…
$ start_lng          <dbl> -87.63645, -87.71000, -87.63754, -87.64445, -87.66801, -87.63754, -87.62619, -87.60727, -87.65814, -87.62232, -87.64754, -87…
$ end_lat            <chr> "41.921822", "41.95", "41.88457623", "41.910522", "41.88577925240433", "41.929143", "41.894722", "41.876243", "41.95469", "4…
$ end_lng            <chr> "-87.64414", "-87.71", "-87.63188991", "-87.653106", "-87.65102461", "-87.649077", "-87.634362", "-87.624426", "-87.67393", …
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "casual", "member", "member", "casual", "member", "mem…
> glimpse(X202304)
Rows: 426,590
Columns: 13
$ ride_id            <chr> "8FE8F7D9C10E88C7", "34E4ED3ADF1D821B", "5296BF07A2F77CB5", "40759916B76D5D52", "77A96F460101AC63", "8D6A2328E19DC168", "C97…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "elec…
$ started_at         <dttm> 2023-04-02 08:37:28, 2023-04-19 11:29:02, 2023-04-19 08:41:22, 2023-04-19 13:31:30, 2023-04-19 12:05:36, 2023-04-19 12:17:3…
$ ended_at           <dttm> 2023-04-02 08:41:37, 2023-04-19 11:52:12, 2023-04-19 08:43:22, 2023-04-19 13:35:09, 2023-04-19 12:10:26, 2023-04-19 12:21:3…
$ start_station_name <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "Kenosha & W…
$ start_station_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "361", NA, N…
$ end_station_name   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "Kenosha…
$ end_station_id     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "361", N…
$ start_lat          <dbl> 41.80, 41.87, 41.93, 41.92, 41.91, 41.91, 41.93, 42.00, 41.99, 41.88, 41.87, 41.79, 41.88, 41.97, 41.96, 41.89, 41.96, 41.97…
$ start_lng          <dbl> -87.60, -87.65, -87.66, -87.65, -87.65, -87.63, -87.66, -87.66, -87.66, -87.65, -87.66, -87.60, -87.65, -87.66, -87.66, -87.…
$ end_lat            <dbl> 41.79, 41.93, 41.93, 41.91, 41.91, 41.92, 41.91, 41.99, 42.00, 41.88, 41.93, 41.79, 41.90, 41.96, 41.97, 41.94, 41.97, 41.96…
$ end_lng            <dbl> -87.60, -87.68, -87.66, -87.65, -87.63, -87.65, -87.65, -87.66, -87.66, -87.65, -87.68, -87.60, -87.63, -87.66, -87.66, -87.…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202305)
Rows: 604,827
Columns: 13
$ ride_id            <chr> "0D9FA920C3062031", "92485E5FB5888ACD", "FB144B3FC8300187", "DDEB93BC2CE9AA77", "C07B70172FC92F59", "2BA66385DF8F815A", "31E…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "classic_bike", "classic_bike", "classic_bike", "docked_bike", "classic_b…
$ started_at         <chr> "5/7/2023 19:53", "5/6/2023 18:54", "5/21/2023 0:40", "5/10/2023 16:47", "5/9/2023 18:30", "5/30/2023 15:01", "5/9/2023 14:1…
$ ended_at           <chr> "5/7/2023 19:58", "5/6/2023 19:03", "5/21/2023 0:44", "5/10/2023 16:59", "5/9/2023 18:39", "5/30/2023 15:17", "5/9/2023 14:4…
$ start_station_name <chr> "Southport Ave & Belmont Ave", "Southport Ave & Belmont Ave", "Halsted St & 21st St", "Carpenter St & Huron St", "Southport …
$ start_station_id   <chr> "13229", "13229", "13162", "13196", "TA1308000047", "TA1305000032", "13300", "TA1308000009", "TA1309000024", "TA1305000032",…
$ end_station_name   <chr> "Not Available", "Not Available", "Not Available", "Damen Ave & Cortland St", "Southport Ave & Belmont Ave", "McClurg Ct & O…
$ end_station_id     <chr> "Not Available", "Not Available", "Not Available", "13133", "13229", "TA1306000029", "13431", "KA1503000070", "TA1307000111"…
$ start_lat          <dbl> 41.93941, 41.93948, 41.85379, 41.89456, 41.95708, 41.88275, 41.88096, 41.79521, 41.90799, 41.88265, 41.88275, 41.90799, 41.9…
$ start_lng          <dbl> -87.66383, -87.66385, -87.64672, -87.65345, -87.66420, -87.64119, -87.61674, -87.58071, -87.63150, -87.64144, -87.64119, -87…
$ end_lat            <dbl> 41.93000, 41.94000, 41.86000, 41.91598, 41.93948, 41.89259, 41.86610, 41.78794, 41.88584, 41.88918, 41.88918, 41.87277, 41.8…
$ end_lng            <dbl> -87.65000, -87.69000, -87.65000, -87.67733, -87.66375, -87.61729, -87.60727, -87.58832, -87.63550, -87.63851, -87.63851, -87…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "casual", "member", "member", "member", "member", "member", "mem…
> glimpse(X202306)
Rows: 719,618
Columns: 13
$ ride_id            <chr> "6F1682AC40EB6F71", "622A1686D64948EB", "3C88859D926253B4", "EAD8A5E0259DEC88", "5A36F21930D6A55C", "CF682EA7D0F961DB", "491…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "elec…
$ started_at         <chr> "6/5/2023 13:34", "6/5/2023 1:30", "6/20/2023 18:15", "6/19/2023 14:56", "6/19/2023 15:03", "6/9/2023 21:30", "6/3/2023 13:3…
$ ended_at           <chr> "6/5/2023 14:31", "6/5/2023 1:33", "6/20/2023 18:32", "6/19/2023 15:00", "6/19/2023 15:07", "6/9/2023 21:49", "6/3/2023 13:3…
$ start_station_name <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ start_station_id   <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ end_station_name   <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ end_station_id     <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ start_lat          <dbl> 41.91, 41.94, 41.95, 41.99, 41.98, 41.99, 41.88, 41.88, 41.95, 41.87, 41.93, 41.89, 41.96, 41.85, 41.98, 41.90, 41.93, 41.93…
$ start_lng          <dbl> -87.69, -87.65, -87.68, -87.65, -87.66, -87.68, -87.62, -87.62, -87.64, -87.63, -87.63, -87.66, -87.66, -87.62, -87.67, -87.…
$ end_lat            <chr> "41.91", "41.94", "41.92", "41.98", "41.99", "41.94", "41.88", "41.88", "41.95", "41.89", "41.94", "41.93", "41.88", "41.86"…
$ end_lng            <chr> "-87.7", "-87.65", "-87.63", "-87.66", "-87.65", "-87.65", "-87.62", "-87.62", "-87.65", "-87.63", "-87.64", "-87.63", "-87.…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202307)
Rows: 767,650
Columns: 13
$ ride_id            <chr> "9340B064F0AEE130", "D1460EE3CE0D8AF8", "DF41BE31B895A25E", "9624A293749EF703", "2F68A6A4CDB4C99A", "9AEE973E6B941A9C", "E36…
$ rideable_type      <chr> "electric_bike", "classic_bike", "classic_bike", "electric_bike", "classic_bike", "classic_bike", "classic_bike", "classic_b…
$ started_at         <chr> "7/23/2023 20:06", "7/23/2023 17:05", "7/23/2023 10:14", "7/21/2023 8:27", "7/8/2023 15:46", "7/10/2023 8:44", "7/25/2023 14…
$ ended_at           <chr> "7/23/2023 20:22", "7/23/2023 17:18", "7/23/2023 10:24", "7/21/2023 8:32", "7/8/2023 15:58", "7/10/2023 8:49", "7/25/2023 14…
$ start_station_name <chr> "Kedzie Ave & 110th St", "Western Ave & Walton St", "Western Ave & Walton St", "Racine Ave & Randolph St", "Clark St & Lelan…
$ start_station_id   <chr> "20204", "KA1504000103", "KA1504000103", "13155", "TA1309000014", "13155", "TA1309000014", "TA1309000014", "TA1309000014", "…
$ end_station_name   <chr> "Public Rack - Racine Ave & 109th Pl", "Milwaukee Ave & Grand Ave", "Damen Ave & Pierce Ave", "Clinton St & Madison St", "Mo…
$ end_station_id     <chr> "877", "13033", "TA1305000041", "TA1305000032", "TA1308000012", "TA1306000015", "TA1307000107", "TA1309000018", "TA130700005…
$ start_lat          <dbl> 41.69241, 41.89842, 41.89842, 41.88411, 41.96709, 41.88407, 41.96709, 41.96709, 41.96709, 42.00455, 41.88033, 41.93175, 41.9…
$ start_lng          <dbl> -87.70091, -87.68660, -87.68660, -87.65694, -87.66729, -87.65685, -87.66729, -87.66729, -87.66749, -87.68067, -87.64275, -87…
$ end_lat            <dbl> 41.69483, 41.89158, 41.90940, 41.88275, 41.96398, 41.88578, 41.96167, 41.95792, 41.93625, 42.00104, 41.86488, 41.92957, 41.9…
$ end_lng            <dbl> -87.65304, -87.64838, -87.67769, -87.64119, -87.63818, -87.65102, -87.65464, -87.67357, -87.65266, -87.66120, -87.64707, -87…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202308)
Rows: 771,693
Columns: 13
$ ride_id            <chr> "903C30C2D810A53B", "F2FB18A98E110A2B", "D0DEC7C94E4663DA", "E0DDDC5F84747ED9", "7797A4874BA260CA", "DF4DE734EBC4DF66", "EE6…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "elec…
$ started_at         <chr> "8/19/2023 15:41", "8/18/2023 15:30", "8/30/2023 16:15", "8/30/2023 16:24", "8/22/2023 15:59", "8/24/2023 12:27", "8/31/2023…
$ ended_at           <chr> "8/19/2023 15:53", "8/18/2023 15:45", "8/30/2023 16:27", "8/30/2023 16:33", "8/22/2023 16:20", "8/24/2023 12:54", "8/31/2023…
$ start_station_name <chr> "LaSalle St & Illinois St", "Clark St & Randolph St", "Clark St & Randolph St", "Wells St & Elm St", "Clark St & Randolph St…
$ start_station_id   <chr> "13430", "TA1305000030", "TA1305000030", "KA1504000135", "TA1305000030", "428", "428", "TA1305000022", "428", "TA1305000022"…
$ end_station_name   <chr> "Clark St & Elm St", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "…
$ end_station_id     <chr> "TA1307000039", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not A…
$ start_lat          <dbl> 41.89072, 41.88451, 41.88498, 41.90310, 41.88555, 41.92467, 41.92471, 41.88779, 41.92474, 41.88799, 41.87585, 41.88472, 41.9…
$ start_lng          <dbl> -87.63148, -87.63155, -87.63079, -87.63467, -87.63202, -87.70060, -87.70062, -87.63688, -87.70061, -87.63704, -87.63113, -87…
$ end_lat            <chr> "41.902973", "41.93", "41.91", "41.9", "41.89", "41.91", "41.91", "41.9", "41.91", "41.89", "41.87", "41.92", "41.91", "41.9…
$ end_lng            <chr> "-87.63128", "-87.64", "-87.63", "-87.62", "-87.68", "-87.72", "-87.72", "-87.63", "-87.67", "-87.62", "-87.62", "-87.68", "…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202309)
Rows: 666,371
Columns: 13
$ ride_id            <chr> "011C1903BF4E2E28", "87DB80E048A1BF9F", "7C2EB7AF669066E3", "57D197B010269CE3", "8A2CEA7C8C8074D8", "03F7044D1304CD58", "672…
$ rideable_type      <chr> "classic_bike", "classic_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "electric_bike", "electric…
$ started_at         <chr> "9/23/2023 0:27", "9/2/2023 9:26", "9/25/2023 18:30", "9/13/2023 15:30", "9/18/2023 15:58", "9/15/2023 20:19", "9/27/2023 16…
$ ended_at           <chr> "9/23/2023 0:33", "9/2/2023 9:38", "9/25/2023 18:41", "9/13/2023 15:39", "9/18/2023 16:05", "9/15/2023 20:30", "9/27/2023 17…
$ start_station_name <chr> "Halsted St & Wrightwood Ave", "Clark St & Drummond Pl", "Financial Pl & Ida B Wells Dr", "Clark St & Drummond Pl", "Halsted…
$ start_station_id   <chr> "TA1309000061", "TA1307000142", "SL-010", "TA1307000142", "TA1309000061", "TA1307000113", "13085", "KA1503000018", "13085", …
$ end_station_name   <chr> "Sheffield Ave & Wellington Ave", "Racine Ave & Fullerton Ave", "Racine Ave & 15th St", "Racine Ave & Belmont Ave", "Racine …
$ end_station_id     <chr> "TA1307000052", "TA1306000026", "13304", "TA1308000019", "TA1306000026", "Not Available", "Not Available", "Not Available", …
$ start_lat          <dbl> 41.92914, 41.93125, 41.87506, 41.93125, 41.92914, 41.92884, 41.92956, 41.76659, 41.92957, 41.92882, 41.87502, 41.99990, 41.9…
$ start_lng          <dbl> -87.64908, -87.64434, -87.63314, -87.64434, -87.64908, -87.66387, -87.70796, -87.57645, -87.70786, -87.66391, -87.63309, -87…
$ end_lat            <dbl> 41.93625, 41.92557, 41.86127, 41.93974, 41.92557, 41.90000, 41.93000, 41.77000, 41.92269, 41.90000, 41.86610, 42.01234, 41.9…
$ end_lng            <dbl> -87.65266, -87.65842, -87.65663, -87.65887, -87.65842, -87.64000, -87.66000, -87.57000, -87.69715, -87.63000, -87.60727, -87…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
> glimpse(X202310)
Rows: 537,113
Columns: 13
$ ride_id            <chr> "4449097279F8BBE7", "9CF060543CA7B439", "667F21F4D6BDE69C", "F92714CC6B019B96", "5E34BA5DE945A9CC", "F7D7420AFAC53CD9", "870…
$ rideable_type      <chr> "classic_bike", "electric_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "electric_bike", "classic…
$ started_at         <chr> "10/8/2023 10:36", "10/11/2023 17:23", "10/12/2023 7:02", "10/24/2023 19:13", "10/9/2023 18:19", "10/4/2023 17:10", "10/31/2…
$ ended_at           <chr> "10/8/2023 10:49", "10/11/2023 17:36", "10/12/2023 7:06", "10/24/2023 19:18", "10/9/2023 18:30", "10/4/2023 17:25", "10/31/2…
$ start_station_name <chr> "Orleans St & Chestnut St (NEXT Apts)", "Desplaines St & Kinzie St", "Orleans St & Chestnut St (NEXT Apts)", "Desplaines St …
$ start_station_id   <chr> "620", "TA1306000003", "620", "TA1306000003", "TA1306000003", "620", "620", "TA1306000003", "13102", "TA1309000064", "TA1309…
$ end_station_name   <chr> "Sheffield Ave & Webster Ave", "Sheffield Ave & Webster Ave", "Franklin St & Lake St", "Franklin St & Lake St", "Franklin St…
$ end_station_id     <chr> "TA1309000033", "TA1309000033", "TA1307000111", "TA1307000111", "TA1307000111", "TA1309000033", "TA1309000033", "TA130700011…
$ start_lat          <dbl> 41.89820, 41.88864, 41.89807, 41.88872, 41.88872, 41.89812, 41.89818, 41.88872, 41.85762, 41.87126, 41.87126, 41.91021, 41.8…
$ start_lng          <dbl> -87.63754, -87.64441, -87.63751, -87.64445, -87.64445, -87.63753, -87.63755, -87.64445, -87.61941, -87.67369, -87.67369, -87…
$ end_lat            <chr> "41.92154", "41.92154", "41.885837", "41.885837", "41.885837", "41.92154", "41.92154", "41.885837", "41.871737", "41.871737"…
$ end_lng            <chr> "-87.653818", "-87.653818", "-87.6355", "-87.6355", "-87.6355", "-87.653818", "-87.653818", "-87.6355", "-87.65103", "-87.65…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "casual", "member", "member", "member", "member", "mem…
> glimpse(X202311)
Rows: 362,518
Columns: 13
$ ride_id            <chr> "4EAD8F1AD547356B", "6322270563BF5470", "B37BDE091ECA38E0", "CF0CA5DD26E4F90E", "EB8381AA641348DB", "B8CF14EA423D6886", "176…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "classic_bike", "classic…
$ started_at         <chr> "11/30/2023 21:50", "11/3/2023 9:44", "11/30/2023 11:39", "11/8/2023 10:01", "11/3/2023 16:20", "11/30/2023 16:15", "11/9/20…
$ ended_at           <chr> "11/30/2023 22:13", "11/3/2023 10:17", "11/30/2023 11:40", "11/8/2023 10:27", "11/3/2023 16:54", "11/30/2023 16:39", "11/9/2…
$ start_station_name <chr> "Millennium Park", "Broadway & Sheridan Rd", "State St & Pearson St", "Theater on the Lake", "Theater on the Lake", "Pine Gr…
$ start_station_id   <chr> "13008", "13323", "TA1307000061", "TA1308000001", "TA1308000001", "TA1307000150", "13008", "TA1308000001", "TA1307000150", "…
$ end_station_name   <chr> "Pine Grove Ave & Waveland Ave", "Broadway & Sheridan Rd", "State St & Pearson St", "Theater on the Lake", "Theater on the L…
$ end_station_id     <chr> "TA1307000150", "13323", "TA1307000061", "TA1308000001", "TA1308000001", "13008", "13008", "TA1308000001", "TA1308000001", "…
$ start_lat          <dbl> 41.88110, 41.95287, 41.89753, 41.92628, 41.92628, 41.94942, 41.88103, 41.92628, 41.94947, 41.94947, 41.94947, 41.94947, 41.9…
$ start_lng          <dbl> -87.62408, -87.65003, -87.62869, -87.63083, -87.63083, -87.64638, -87.62408, -87.63083, -87.64645, -87.64645, -87.64645, -87…
$ end_lat            <chr> "41.94947274088333", "41.952833", "41.897448", "41.926277", "41.926277", "41.8810317", "41.8810317", "41.926277", "41.926277…
$ end_lng            <chr> "-87.64645278", "-87.649993", "-87.628722", "-87.630834", "-87.630834", "-87.62408432", "-87.62408432", "-87.630834", "-87.6…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "casual", "member", "member", "member", "member", "member", "mem…
> glimpse(X202312)
Rows: 224,073
Columns: 13
$ ride_id            <chr> "C9BD54F578F57246", "CDBD92F067FA620E", "ABC0858E52CBFC84", "F44B6F0E8F76DC90", "3C876413281A90DF", "28C0D6EFB81E1769", "8A3…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "elec…
$ started_at         <chr> "12/2/2023 18:44", "12/2/2023 18:48", "12/24/2023 1:56", "12/24/2023 10:58", "12/24/2023 12:43", "12/24/2023 13:59", "12/24/…
$ ended_at           <chr> "12/2/2023 18:47", "12/2/2023 18:54", "12/24/2023 2:04", "12/24/2023 11:03", "12/24/2023 12:44", "12/24/2023 14:10", "12/24/…
$ start_station_name <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ start_station_id   <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ end_station_name   <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ end_station_id     <chr> "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not Available", "Not …
$ start_lat          <dbl> 41.92, 41.92, 41.89, 41.95, 41.92, 41.91, 41.99, 42.00, 41.96, 41.90, 41.95, 41.94, 41.94, 41.89, 41.93, 41.93, 41.98, 41.91…
$ start_lng          <dbl> -87.66, -87.66, -87.62, -87.65, -87.64, -87.63, -87.68, -87.67, -87.68, -87.63, -87.65, -87.64, -87.64, -87.62, -87.71, -87.…
$ end_lat            <dbl> 41.92, 41.89, 41.90, 41.94, 41.93, 41.88, 42.00, 41.99, 41.97, 41.90, 41.96, 41.94, 41.94, 41.93, 41.89, 41.91, 41.98, 41.92…
$ end_lng            <dbl> -87.66, -87.64, -87.64, -87.65, -87.64, -87.65, -87.67, -87.68, -87.68, -87.63, -87.65, -87.64, -87.64, -87.72, -87.62, -87.…
$ member_casual      <chr> "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "member", "mem…
```
Two columns, started_at and ended_at, have different formats.  As I will be using them for calculations, it is important for them to all be the same.  So the next step is to make them all dttm format in yyyy-mm-dd hh:mm:ss format
```r
# format chr timestamp as dttm
> X202301$started_at <- mdy_hm(X202301$started_at)
> X202301$ended_at <- mdy_hm(X202301$ended_at)
> X202302$started_at <- mdy_hm(X202302$started_at)
> X202302$ended_at <- mdy_hm(X202302$ended_at)
> X202303$started_at <- mdy_hm(X202303$started_at)
> X202303$ended_at <- mdy_hm(X202303$ended_at)
> X202305$started_at <- mdy_hm(X202305$started_at)
> X202305$ended_at <- mdy_hm(X202305$ended_at)
> X202306$started_at <- mdy_hm(X202306$started_at)
> X202306$ended_at <- mdy_hm(X202306$ended_at)
> X202307$started_at <- mdy_hm(X202307$started_at)
> X202307$ended_at <- mdy_hm(X202307$ended_at)
> X202308$started_at <- mdy_hm(X202308$started_at)
> X202308$ended_at <- mdy_hm(X202308$ended_at)
> X202309$started_at <- mdy_hm(X202309$started_at)
> X202309$ended_at <- mdy_hm(X202309$ended_at)
> X202310$started_at <- mdy_hm(X202310$started_at)
> X202310$ended_at <- mdy_hm(X202310$ended_at)
> X202311$started_at <- mdy_hm(X202311$started_at)
> X202311$ended_at <- mdy_hm(X202311$ended_at)
> X202312$started_at <- mdy_hm(X202312$started_at)
> X202312$ended_at <- mdy_hm(X202312$ended_at)
```
I also discovered the four columns for latitude and longitude also have different formats.  
```r
# changes format for the longitude and latitude as numeric
X202303 <-  mutate(X202303, end_lat = as.numeric(end_lat)) 
X202303 <-  mutate(X202303, end_lng = as.numeric(end_lng))
X202306 <-  mutate(X202306, end_lat = as.numeric(end_lat)) 
X202306 <-  mutate(X202306, end_lng = as.numeric(end_lng)) 
X202308 <-  mutate(X202308, end_lat = as.numeric(end_lat)) 
X202308 <-  mutate(X202308, end_lng = as.numeric(end_lng)) 
X202310 <-  mutate(X202310, end_lat = as.numeric(end_lat)) 
X202310 <-  mutate(X202310, end_lng = as.numeric(end_lng))
X202311 <-  mutate(X202311, end_lat = as.numeric(end_lat)) 
X202311 <-  mutate(X202311, end_lng = as.numeric(end_lng))
```
Now we are finally ready to combine the 12 data frames into one stack
```r
# Stack individual quarter's data frames into one big data frame
all_trips_v1 <- bind_rows(X202301, X202302, X202303, X202304, X202305, X202306, X202307, X202308, X202309, X202310, X202311, X202312)
```
## 2.9 Review Combined all_trips data frame

Before proceeding, I went ahead and took a look at the newly combined data frame
```r
colnames(all_trips_v1)  #List of column names
dim(all_trips_v1)  #Dimensions of the data frame?
head(all_trips_v1)  #See the first 6 rows of data frame. Also tail(all_trips)
str(all_trips_v1)  #See list of columns and data types (numeric, character, etc)
summary(all_trips_v1)  #Statistical summary of data. Mainly for numerics

> colnames(all_trips_v1)  #List of column names
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"     
> dim(all_trips_v1)  #Dimensions of the data frame?
[1] 5719877      13
> head(all_trips_v1)  #See the first 6 rows of data frame. Also tail(all_trips)
# A tibble: 6 × 13
  ride_id   rideable_type started_at          ended_at            start_station_name start_station_id end_station_name end_station_id start_lat start_lng
  <chr>     <chr>         <dttm>              <dttm>              <chr>              <chr>            <chr>            <chr>              <dbl>     <dbl>
1 F96D5A74… electric_bike 2023-01-21 20:05:00 2023-01-21 20:16:00 Lincoln Ave & Ful… TA1309000058     Hampden Ct & Di… 202480              41.9     -87.6
2 13CB7EB6… classic_bike  2023-01-10 15:37:00 2023-01-10 15:46:00 Kimbark Ave & 53r… TA1309000037     Greenwood Ave &… TA1308000002        41.8     -87.6
3 BD88A2E6… electric_bike 2023-01-02 07:51:00 2023-01-02 08:05:00 Western Ave & Lun… RP-005           Valli Produce -… 599                 42.0     -87.7
4 C90792D0… classic_bike  2023-01-22 10:52:00 2023-01-22 11:01:00 Kimbark Ave & 53r… TA1309000037     Greenwood Ave &… TA1308000002        41.8     -87.6
5 33970175… classic_bike  2023-01-12 13:58:00 2023-01-12 14:13:00 Kimbark Ave & 53r… TA1309000037     Greenwood Ave &… TA1308000002        41.8     -87.6
6 58E68156… electric_bike 2023-01-31 07:18:00 2023-01-31 07:21:00 Lakeview Ave & Fu… TA1309000019     Hampden Ct & Di… 202480              41.9     -87.6
# ℹ 3 more variables: end_lat <dbl>, end_lng <dbl>, member_casual <chr>
> str(all_trips_v1)  #See list of columns and data types (numeric, character, etc)
spc_tbl_ [5,719,877 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ ride_id           : chr [1:5719877] "F96D5A74A3E41399" "13CB7EB698CEDB88" "BD88A2E670661CE5" "C90792D034FED968" ...
 $ rideable_type     : chr [1:5719877] "electric_bike" "classic_bike" "electric_bike" "classic_bike" ...
 $ started_at        : POSIXct[1:5719877], format: "2023-01-21 20:05:00" "2023-01-10 15:37:00" "2023-01-02 07:51:00" "2023-01-22 10:52:00" ...
 $ ended_at          : POSIXct[1:5719877], format: "2023-01-21 20:16:00" "2023-01-10 15:46:00" "2023-01-02 08:05:00" "2023-01-22 11:01:00" ...
 $ start_station_name: chr [1:5719877] "Lincoln Ave & Fullerton Ave" "Kimbark Ave & 53rd St" "Western Ave & Lunt Ave" "Kimbark Ave & 53rd St" ...
 $ start_station_id  : chr [1:5719877] "TA1309000058" "TA1309000037" "RP-005" "TA1309000037" ...
 $ end_station_name  : chr [1:5719877] "Hampden Ct & Diversey Ave" "Greenwood Ave & 47th St" "Valli Produce - Evanston Plaza" "Greenwood Ave & 47th St" ...
 $ end_station_id    : chr [1:5719877] "202480" "TA1308000002" "599" "TA1308000002" ...
 $ start_lat         : num [1:5719877] 41.9 41.8 42 41.8 41.8 ...
 $ start_lng         : num [1:5719877] -87.6 -87.6 -87.7 -87.6 -87.6 ...
 $ end_lat           : num [1:5719877] 41.9 41.8 42 41.8 41.8 ...
 $ end_lng           : num [1:5719877] -87.6 -87.6 -87.7 -87.6 -87.6 ...
 $ member_casual     : chr [1:5719877] "member" "member" "casual" "member" ...
 - attr(*, "spec")=
  .. cols(
  ..   ride_id = col_character(),
  ..   rideable_type = col_character(),
  ..   started_at = col_character(),
  ..   ended_at = col_character(),
  ..   start_station_name = col_character(),
  ..   start_station_id = col_character(),
  ..   end_station_name = col_character(),
  ..   end_station_id = col_character(),
  ..   start_lat = col_double(),
  ..   start_lng = col_double(),
  ..   end_lat = col_double(),
  ..   end_lng = col_double(),
  ..   member_casual = col_character()
  .. )
 - attr(*, "problems")=<externalptr> 
> summary(all_trips_v1)  #Statistical summary of data. Mainly for numerics
   ride_id          rideable_type        started_at                       ended_at                      start_station_name start_station_id  
 Length:5719877     Length:5719877     Min.   :2023-01-01 00:01:00.0   Min.   :2023-01-01 00:02:00.00   Length:5719877     Length:5719877    
 Class :character   Class :character   1st Qu.:2023-05-21 12:50:00.0   1st Qu.:2023-05-21 13:14:00.00   Class :character   Class :character  
 Mode  :character   Mode  :character   Median :2023-07-20 18:02:00.0   Median :2023-07-20 18:19:00.00   Mode  :character   Mode  :character  
                                       Mean   :2023-07-16 10:27:22.7   Mean   :2023-07-16 10:45:32.86                                        
                                       3rd Qu.:2023-09-16 20:08:00.0   3rd Qu.:2023-09-16 20:28:00.00                                        
                                       Max.   :2023-12-31 23:59:00.0   Max.   :2024-01-01 23:50:00.00                                        
                                                                                                                                             
 end_station_name   end_station_id       start_lat       start_lng         end_lat         end_lng       member_casual     
 Length:5719877     Length:5719877     Min.   :41.63   Min.   :-87.94   Min.   : 0.00   Min.   :-88.16   Length:5719877    
 Class :character   Class :character   1st Qu.:41.88   1st Qu.:-87.66   1st Qu.:41.88   1st Qu.:-87.66   Class :character  
 Mode  :character   Mode  :character   Median :41.90   Median :-87.64   Median :41.90   Median :-87.64   Mode  :character  
                                       Mean   :41.90   Mean   :-87.65   Mean   :41.90   Mean   :-87.65                     
                                       3rd Qu.:41.93   3rd Qu.:-87.63   3rd Qu.:41.93   3rd Qu.:-87.63                     
                                       Max.   :42.07   Max.   :-87.46   Max.   :42.18   Max.   :  0.00                     
                                                                        NA's   :6990    NA's   :6990    
```
In order to aggregate trip data for each month or day, we will add a few columns.
```r
# Add columns that list the date, month, day, and year of each ride
all_trips_v1$date <- as.Date(all_trips_v1$started_at) #The default format is yyyy-mm-dd
all_trips_v1$month <- format(as.Date(all_trips_v1$date), "%B") #Displays the month as month.name
all_trips_v1$day <- format(as.Date(all_trips_v1$date), "%d")
all_trips_v1$year <- format(as.Date(all_trips_v1$date), "%Y")
all_trips_v1$day_of_week <- format(as.Date(all_trips_v1$date), "%A")

> colnames(all_trips_v1)  #List of column names
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"   "end_station_name"  
 [8] "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"            "member_casual"      "date"              
[15] "month"              "day"                "year"               "day_of_week"   
```
To be able to analyze rider usage differences in terms of ride lengths, I will now add a calculating the difference between start and end times and convert the result to a numeric value to allow for calculations and visuals.
```r
# Add a "ride_length" calculation to all_trips (in seconds)
all_trips_v1$ride_length <- difftime(all_trips_v1$ended_at,all_trips_v1$started_at)

# Convert "ride_length" from Factor to numeric so we can run calculations on the data
is.factor(all_trips_v1$ride_length)
all_trips_v1$ride_length <- as.numeric(as.character(all_trips_v1$ride_length))
is.numeric(all_trips_v1$ride_length)

> all_trips_v1$ride_length <- difftime(all_trips_v1$ended_at,all_trips_v1$started_at)
> is.factor(all_trips_v1$ride_length)
[1] FALSE
> all_trips_v1$ride_length <- as.numeric(as.character(all_trips_v1$ride_length))
> is.numeric(all_trips_v1$ride_length)
[1] TRUE
```

Let's do a summary of the ride length column.  The median is 600 seconds (10 minutes), but the max is 5909340 seconds which is over 68 days.  That seems invalid.  
```r
> summary(all_trips_v1$ride_length)
   Min.     1st Qu.  Median    Mean  3rd Qu.    Max. 
  -999420     300     598    1090    1020     5909340
```
to get a better fell for ride times, I chose to convert this to minutes
```r
> all_trips_v2$ride_length_mins <- all_trips_v2$ride_length/60
> summary(all_trips_v1$ride_length_mins)
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
-16657.00      5.00      9.97     18.17     17.00  98489.00
```

After seeing that there are some ride length times with negative values, we will need to eliminate those rows as being bad data. I am also going to eliminate rides that were less than 2 mins as being false starts or rides used to move to different dock.
As I am removing data, I created a new data frame all_trips_v2
```r
# Remove "bad" data
# We will create a new version of the dataframe (v2) since data is being removed

all_trips_v2 <- all_trips_v2 %>% 
  filter(ride_length_mins >= 2 & ride_length_mins <= 1440)

```

Earlier, I added a column for month, but I also want to see how rides are different by season and time of day so I will add two more columns to track those. 
```r
# create a column to assign seasons to the months
all_trips_v2 <- all_trips_v2 %>% mutate(seasons = recode(month_name,
                                                    December = "Winter",
                                                    January = "Winter",
                                                    February = "Winter",
                                                    March = "Spring",
                                                    April = "Spring",
                                                    May = "Spring",
                                                    June = "Summer",
                                                    July = "Summer",
                                                    August = "Summer",
                                                    September = "Fall",
                                                    October = "Fall",
                                                    November = "Fall"))
```
We will want to be able to see how riders use the bike during different times of the day so I have assigned the hour to periods of the day 
```r
#create a column for time of day from the hour column created earlier 
all_trips_v2 <- all_trips_v2 %>% mutate(time_of_day = case_when(
  hour >= 4 & hour < 8 ~ "Early Morning",
  hour >= 8 & hour < 12 ~ "Mid Morning",
  hour >= 12 & hour < 16  ~ "Afternoon",
  hour >= 16 & hour < 20  ~ "Evening",
  hour >= 20 & hour <= 23  ~ "Early Night",
  hour >= 0 & hour < 2 ~ "Early Night",
  hour >= 2 & hour < 6  ~ "Late Night"))
```
## 2.10 Final verification of data before starting analysis
Duplicate Data: Data has no duplicates
Missing Values: elected to keep rows with missing station and lat/lng as the rows involved were insignificant
Outliers: Removed rides shorter than 2 mins as being invalid (test or bike removed for service) and rides over 24 hours
General: After processing, the data is complete, relevant, consistent and accurate.

# 3 ANALYZE

## 3.1 Comparing Number of Rides - Member vs. Casual

Let's start by getting a feel for how many casual rides there were compared to member rides.  Note this ONLY compare rides as each rider could have multiple rides.  We can see that 64% of rides for 2023 were taken by members.
```r
# Count and percentage of member vs. casual riders
all_trips_v2 %>% 
  group_by(member_casual) %>% 
  summarize(count = n(), Percentage = n()/nrow(all_trips_v2)*100)

> all_trips_v2 %>% 
+   group_by(member_casual) %>% 
+   summarize(count = n(), Percentage = n()/nrow(all_trips_v2)*100)
# A tibble: 2 × 3
  member_casual   count Percentage
  <chr>           <int>      <dbl>
1 casual        1984491       36.0
2 member        3523930       64.0
```

## 3.2 Compare Ride Lengths - Member vs. Casual

Let's compare the mean, median, min, and max ride lengths for members and casual riders
```r
# Compare members and casual users
aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = mean)
aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = median)
aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = max)
aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = min)

> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = mean)
  all_trips_v2$member_casual all_trips_v2$ride_length_mins
1                     casual                      21.39670
2                     member                      12.49711
> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = median)
  all_trips_v2$member_casual all_trips_v2$ride_length_mins
1                     casual                            12
2                     member                             9
> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = max)
  all_trips_v2$member_casual all_trips_v2$ride_length_mins
1                     casual                          1440
2                     member                          1440
> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual, FUN = min)
  all_trips_v2$member_casual all_trips_v2$ride_length_mins
1                     casual                             2
2                     member                             2
```
We can immediately see that casual riders take longer rides, which is at least partially skewed as there is an outlier highlighted in the max time.  We will want to investigate further to see how many other ride times are like this.  It could be the only one, indicating a possible data issue or it could signify a portion of users who rent the bikes and keep them for over 24 hours.    

## 3.2 Compare Daily Avg Ride Times - Members vs. Casual

Let's take a look how members and casual riders differ in per day.
```r
# Member/casual average ride times for each day
> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
   all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length_mins
1                      casual                   Friday                      20.77405
2                      member                   Friday                      12.43841
3                      casual                   Monday                      21.00054
4                      member                   Monday                      11.86208
5                      casual                 Saturday                      24.25643
6                      member                 Saturday                      13.93706
7                      casual                   Sunday                      24.98214
8                      member                   Sunday                      13.93531
9                      casual                 Thursday                      18.65148
10                     member                 Thursday                      12.00116
11                     casual                  Tuesday                      19.13015
12                     member                  Tuesday                      11.99702
13                     casual                Wednesday                      18.24386
14                     member                Wednesday                      11.91430
```
Again we see that casual members have longer rides than members every day.  This could indicate that members may be taking advantage of unlimited rides to use the bikes for work commute or even local errands and could mean casual riders may be more likely to be taking in scenery.  While the data does not contain information to help us know how many riders are local versus visitors, it is safe to assume that the casual riders have a higher percentage of non-local riders, which could be attributing to the longer ride times for casual riders.

That said, looking at the data above, the days of the week are out of order.  To help us identify trends, I will put them in order (starting with Monday so that the weekend data stays together)
```r
> all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))
> aggregate(all_trips_v2$ride_length_mins ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
   all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length_mins
1                      casual                   Monday                      21.00054
2                      member                   Monday                      11.86208
3                      casual                  Tuesday                      19.13015
4                      member                  Tuesday                      11.99702
5                      casual                Wednesday                      18.24386
6                      member                Wednesday                      11.91430
7                      casual                 Thursday                      18.65148
8                      member                 Thursday                      12.00116
9                      casual                   Friday                      20.77405
10                     member                   Friday                      12.43841
11                     casual                 Saturday                      24.25643
12                     member                 Saturday                      13.93706
13                     casual                   Sunday                      24.98214
14                     member                   Sunday                      13.93531

```
Once they are in order, we can also now see that the weekends have longer rides than the week days for casual riders and members.  

## 3.3 Create weekly_trips data frame to summarize member vs. casual usage on different days

Let's look at a summary of rides by day showing number of rides per day and average length per day, grouped by the rider types (member, casual)

```r
# create new DF to summarize daily usage
weekly_trips <- all_trips_v2 %>%
  group_by(member_casual, day_of_week) %>%
  summarize(total_trips = n(), avg_trip_mins = mean(ride_length_mins))

head(weekly_trips, 14)
> weekly_trips <- all_trips_v2 %>%
+   group_by(member_casual, day_of_week) %>%
+   summarize(total_trips = n(), avg_trip_mins = mean(ride_length_mins))
`summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.
> head(weekly_trips, 14)
# A tibble: 14 × 4
# Groups:   member_casual [2]
   member_casual day_of_week total_trips avg_trip_mins
   <chr>         <ord>             <int>         <dbl>
 1 casual        Monday           226559          21.0
 2 casual        Tuesday          237452          19.1
 3 casual        Wednesday        240351          18.2
 4 casual        Thursday         260981          18.7
 5 casual        Friday           300599          20.8
 6 casual        Saturday         395404          24.3
 7 casual        Sunday           323145          25.0
 8 member        Monday           476224          11.9
 9 member        Tuesday          555546          12.0
10 member        Wednesday        565275          11.9
11 member        Thursday         567276          12.0
12 member        Friday           511156          12.4
13 member        Saturday         454932          13.9
14 member        Sunday           393521          13.9


```
Looking at this summary, we can see stark differences in the usage, especially comparing weekday to weekend volume.  For casual riders, the weekends see the highest number and the longest rides.  For members, there is a significant drop in the number of rideson the weekends, though the average per ride is increased.  That dropoff on the weekends for members is further evidence that many members use the bikes for work.

Let's plot both the number of rides by rider type and the average length of rides by rider type
```r
# create new DF to summarize daily usage
weekly_trips <- all_trips_v2 %>%
  group_by(member_casual, day_of_week) %>%
  summarize(total_trips = n(), avg_trip_mins = mean(ride_length_mins))

# plot for number of trips by day and rider type
ggplot(data = weekly_trips)+
  geom_col(mapping = aes(x = day_of_week, y = total_trips, fill = member_casual))+
  theme(axis.text.x = element_text(angle = 45))+
  labs(title = 'Rides by Day of Week 2023', subtitle = 'Comparing Casual Riders to Members', x = 'Day of the Week', y = 'Rides', fill = 'Member Type')+
  facet_wrap(~member_casual)
```
![This is an image](https://i.imgur.com/rzuZM34.png)

We can see that for Casual riders the number of rides is fairly consistent Monday - Thursday, spiking as it gets to Friday - Sunday.  We would need other data to verify if that increase is due to additional non-local visitors on the weekends.
We also see the reverse on Member usage.  They have more rides Monday - Friday with a sharp decline on the weekends.  There is suspicion that these trends are related to more tourists on the weekend, driving the casual increase along with more work-related bike usage for members.  

```r
# plot for average ride length by day of week and rider type
ggplot(data = weekly_trips)+
  geom_col(mapping = aes(x = day_of_week, y = avg_trip_mins, fill = member_casual))+
  theme(axis.text.x = element_text(angle = 45))+
  labs(title = 'Average Ride in Minutes by Day 2023', subtitle = 'Comparing Casual Riders with Members', x = 'Day of the Week', y = 'Averageg Ride in Minutes', fill = 'Member Type')+
  facet_wrap(~member_casual)
```
![This is an image](https://i.imgur.com/4nWxQF9.png)

When we look at the average ride times, the members data is remarkably flat with little variance from day to day but with a slight increase on the weekends, where the data for the casual riders vary considerably from day to day, but especially increase with longer rides on the weekends.  

Now let's look at a plot for rides by bike type and rider type.  
```r
# create new DF to summarize bike type usage
biketype_trips <- all_trips_v2 %>%
  group_by(member_casual, rideable_type) %>%
  summarize(total_rt_trips = n(), avg_rt_trip_mins = mean(ride_length_mins))

# plot for number of trips bike type
ggplot(data = biketype_trips)+
  geom_col(mapping = aes(x = rideable_type, y = total_rt_trips, fill = member_casual))+
  theme(axis.text.x = element_text(angle = 45))+
  labs(title = 'Rides by Bike Type 2023', subtitle = 'Comparing Casual Riders to Members', x = 'Member Type', y = 'Rides', fill = 'Member Type')+
  facet_wrap(~member_casual)
```
![This is an image](https://i.imgur.com/iOqbL7r.png)

The data tells us that members are pretty evenly split between the electric and classic bikes, but the casual rider has a slight preference for the electric bike.  While the available data cannot verify why, my leading assumption is condition of the rider groups.  As visitors may include a portion of customers who are not used to riding a bike, they might opt for more electric options to conserve energy.

## 3.4 Compare Ride Intervals - Member vs. Casual

Let's look at length of rides in 30 minute intervals by rider type. The data shows almost 5 million rides (casual 1644790, members 3311582) were less than 30 minutes.  Another interesting point is that members comprised 66.8% of rides under 30 minutes, while that dipped to 45% of rides between 30 and 60 minutes and only 18.6% of rides over 60 minutes.  That data makes it quite clear that member rides are typically short
```r
all_trips_v2 %>%
  group_by(member_casual) %>%
  summarize("<30 min" = sum(ride_length_mins <30.00),
            "30-59 min" = sum(ride_length_mins >=30 & ride_length_mins <60.00),
            "60-89 min" = sum(ride_length_mins >=60 & ride_length_mins <90.00),
            "90-119 min" = sum(ride_length_mins >=90 & ride_length_mins <120.00),
            ">120 min" = sum(ride_length_mins >=120)
  )

> all_trips_v2 %>%
+   group_by(member_casual) %>%
+   summarize("<30 min" = sum(ride_length_mins <30.00),
+             "30-59 min" = sum(ride_length_mins >=30 & ride_length_mins <60.00),
+             "60-89 min" = sum(ride_length_mins >=60 & ride_length_mins <90.00),
+             "90-119 min" = sum(ride_length_mins >=90 & ride_length_mins <120.00),
+             ">120 min" = sum(ride_length_mins >=120)
+   )
# A tibble: 2 × 6
  member_casual `<30 min` `30-59 min` `60-89 min` `90-119 min` `>120 min`
  <chr>             <int>       <int>       <int>        <int>      <int>
1 casual          1644790      228479       61200        23583      26439
2 member          3311582      186953       13597         3933       7865
```
![This is an image](https://i.imgur.com/MN3nMxi.png)


Being curious, I wondered what 20 minute intervals would look like
```r
all_trips_v2 %>%
  group_by(member_casual) %>%
  summarize("<20 min" = sum(ride_length_mins <20.00),
            "20-39 min" = sum(ride_length_mins >=20 & ride_length_mins <40.00),
            "40-59 min" = sum(ride_length_mins >=40 & ride_length_mins <60.00),
            "60-79 min" = sum(ride_length_mins >=60 & ride_length_mins <80.00),
            "80-99 min" = sum(ride_length_mins >=80 & ride_length_mins <100.00),
            "100-119 min" = sum(ride_length_mins >=100 & ride_length_mins <120.00),
            ">120 min" = sum(ride_length_mins >=120)
  )

> all_trips_v2 %>%
+   group_by(member_casual) %>%
+   summarize("<20 min" = sum(ride_length_mins <20.00),
+             "20-39 min" = sum(ride_length_mins >=20 & ride_length_mins <40.00),
+             "40-59 min" = sum(ride_length_mins >=40 & ride_length_mins <60.00),
+             "60-79 min" = sum(ride_length_mins >=60 & ride_length_mins <80.00),
+             "80-99 min" = sum(ride_length_mins >=80 & ride_length_mins <100.00),
+             "100-119 min" = sum(ride_length_mins >=100 & ride_length_mins <120.00),
+             ">120 min" = sum(ride_length_mins >=120)
+   )
# A tibble: 2 × 8
  member_casual `<20 min` `20-39 min` `40-59 min` `60-79 min` `80-99 min` `100-119 min` `>120 min`
  <chr>             <int>       <int>       <int>       <int>       <int>         <int>      <int>
1 casual          1385444      380235      107590       47263       24037         13483      26439
2 member          2977922      457789       62824       11003        4347          2180       7865

```
These visuals illustrate the difference at 20 mins compared to all rides longer than 20 minutes.
![This is an image](https://i.imgur.com/aloNjZS.png)

## 3.5 Compare rides by Month and Season - Member vs. Casual

Now let's look at rides by a few different break downs
Let's start with rides by month.  When we look at this information by season shortly you will see even more dramatic results, but here you can easily see that unsurprisingly the winter month (Dec-Feb) have the lowest usage, and the summer months (Jun-Aug) have the highest usage by both members and casual riders.
```r
all_trips_v2$month = factor(all_trips_v2$month, levels = month.name)
ggplot(all_trips_v2, aes(x = month)) +
  geom_bar(aes(fill = member_casual)) +
  ggtitle("Rides by month") +
  xlab("Month") + ylab("Number of rides")
```
![This is an image](https://i.imgur.com/k8XUPNH.png)


```r
ggplot(all_trips_v2, aes(x = season)) +
  geom_bar(aes(fill = member_casual)) +
  ggtitle("Rides by Season") +
  xlab("Season") + ylab("Number of rides")
```
![This is an image](https://i.imgur.com/3NFSDNp.png)



