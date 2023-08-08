---
title: 'Cyclistic Bike Share Analysis'
author: "Michelle Penner"
email: "pennermf@gmail.com"
date: "2023-08-05"
output: html_document
---
  
## Summary of Analysis for Cyclistic Bike Share
  
Annually, members ride more frequently than casual users. This suggests that members are more regular and frequent users of the bike-sharing service compared to casual users.

Tuesday to Thursday are the most popular days for members, indicating a regular weekday commuting pattern. This aligns with the idea that members use the service primarily for daily commuting purposes during the workweek.

Both members and casual users ride most often between 3-6 p.m., indicating high demand during the late afternoon hours. This peak in usage could be attributed to people using the bikes for leisure or running errands in the late afternoon.

The top start and end stations for members are Kingsbury and Kinzie St., while casual users prefer Streeter Dr. and Grand Ave. This difference in station preferences may be related to the locations of residential and commercial areas frequented by members and casual users.

Casual users predominantly ride on Saturday and Sunday, indicating a preference for weekends. This suggests that casual users use the service more for leisure or recreational activities during the weekends.

Casual users ride longer distances on average compared to members. This difference in ride distances might imply that casual users prefer longer rides, exploring various destinations, or using the service for leisurely city tours.

July records the highest ridership, while December has the lowest. This seasonal variation might be due to weather conditions, as more people tend to use bike-sharing services during pleasant weather.

Saturday is the most popular day of the week across all users and is the only day casual users slightly surpass members in usage. This indicates that Saturdays are the busiest days for both groups, and casual users might have a slightly higher usage on Saturdays compared to other weekdays.

## My 3 Recommendations Based On My Analysis
The marketing strategy should focus on increasing membership awareness and attracting casual users by offering special deals in locations where casual users are most frequent. By targeting specific areas, the marketing efforts can be more effective while reducing overall expenditure.

Incentives should be designed to encourage casual riders to transition to becoming members. Emphasize the benefits of membership, such as reducing one carbon footprint, saving money, and improving health through low-impact physical activity. These angles can resonate with potential members and motivate them to make the switch.

Conduct targeted surveys for all user types to gain valuable insights. By using data-driven user type hotspots, the surveys can gather detailed feedback from different user segments. This information will help in understanding user preferences, needs, and pain points, allowing for more informed decision-making and tailored marketing strategies.

## How do annual members and casual riders use Cyclistic differently?
The provided information pertains specifically to the difference between annual members and casual riders during the period from July 2022 to June 2023. It is important to note that these insights are limited to the data we have on hand, and further implications may require addressing the remaining stakeholder questions.

Here is a summary of the key findings:
  
*Members prefer riding on weekdays, with the most frequent rides occurring from Tuesday to Thursday. In contrast, casual users prefer weekends, particularly Saturdays, suggesting different usage patterns for both groups.
*Kingsbury and Kinzie St. are the top start and end stations for members, whereas Streeter Dr. and Grand Ave. are preferred by casual users. Station preferences may align with the locations of residential and commercial areas frequented by each group.
*Classic bikes are the most preferred bike type among both members and casual users, indicating a preference for traditional biking options.
*Both groups commonly ride between 3-6 p.m., likely corresponding to the late afternoon when many people use the service for various activities.
*April to October is the preferred period for both members and casual users, likely due to favorable weather conditions during these months.
*July is the most popular month, likely due to summer weather and increased outdoor activities. December records the lowest ridership, possibly due to colder weather and holiday-related factors.
*Saturday is the most popular day of the week across all users, and it is the only day when casual users slightly surpass members in usage, although the difference is marginal.
*It is important to consider these findings when formulating strategies and making data-driven decisions to better serve both member and casual user segments effectively.

The data has been made available by Motivate International Inc. under this [License](https://ride.divvybikes.com/data-license-agreement).

## Data Wrangling Change Log

* Date Range: 2022-07-01 to 2023-06-30

August 5, 2023:
  
* Total Rows: 5,779,444
* I raised the following concern (about the data) with Lily Moreno(The director of marketing): 
858,403 rows with missing Starting Station names (and ID),

* I was advised by Lily Moreno that we could ignore rows with trip duraton <=0.
We could also ignore rows with missing start_station_name, and also ignore stations_id. 
* And should focus on doing using the start_station_name to preform aggregate functions on date, start_station_name, member_casual and rideable_type.
* Cleaning reducted rows to 4,921,041


```{r setup, include=FALSE,echo=FALSE,error=FALSE,message=FALSE}
knitr::opts_chunk$set(echo = FALSE,message = FALSE,error = FALSE)
library(tidyverse)
library(janitor)
library(lubridate)
library(scales)
rm(list=ls())
```

## Import previous 12 months data

```{r}
df1 <- read.csv("202306-divvy-tripdata.csv")
df2 <- read.csv("202305-divvy-tripdata.csv")
df3 <- read.csv("202304-divvy-tripdata.csv")
df4 <- read.csv("202303-divvy-tripdata.csv")
df5 <- read.csv("202302-divvy-tripdata.csv")
df6 <- read.csv("202301-divvy-tripdata.csv")
df7 <- read.csv("202212-divvy-tripdata.csv")
df8 <- read.csv("202211-divvy-tripdata.csv")
df9 <- read.csv("202210-divvy-tripdata.csv")
df10 <- read.csv("202209-divvy-tripdata.csv")
df11 <- read.csv("202208-divvy-tripdata.csv")
df12 <- read.csv("202207-divvy-tripdata.csv")
```
## Combine 12 data.frames into One (1) data.frame

```{r}
bike_rides <- rbind(df1,df2,df3,df4,df5,df6,df7,df8,df9,df10,df11,df12)
```
### Clean data

```{r}
bike_rides <- janitor ::remove_empty(bike_rides,which = c("cols"))
bike_rides <- janitor::remove_empty(bike_rides,which = c("rows"))
bike_rides <- bike_rides  %>% filter(start_station_name !="")
```
## Convert Data/Time stamp to Date/Time 

```{r}
bike_rides$Ymd <- as.Date(bike_rides$started_at)
bike_rides$started_at <- lubridate::ymd_hms(bike_rides$started_at)
bike_rides$ended_at <- lubridate::ymd_hms(bike_rides$ended_at)

bike_rides$start_hour <- lubridate::hour(bike_rides$started_at)
bike_rides$end_hour <- lubridate::hour(bike_rides$ended_at)
```
### Create new columns for duration of each bike ride

```{r}
bike_rides$Hours <- difftime(bike_rides$ended_at,bike_rides$started_at,units = c("hours"))

bike_rides$Minutes <- difftime(bike_rides$ended_at,bike_rides$started_at,units = c("mins"))

bike_rides <- bike_rides %>% filter(Minutes >0)
```
### Inspect the new table that has been created

```{r}
colnames(bike_rides)  #List of column names
nrow(bike_rides)  #How many rows are in data frame?
dim(bike_rides)  #Dimensions of the data frame?
head(bike_rides)  #See the first 6 rows of data frame.  Also tail(bike_rides)
str(bike_rides)  #See list of columns and data types (numeric, character, etc)
summary(bike_rides)  #Statistical summary of data. Mainly for numeric
```

### Create summary data frame

```{r}
bikesrides2 <- bike_rides %>% 
  group_by(Weekly = floor_date(started_at, "week"), start_hour) %>%
  summarise(
    Minutes = sum(Minutes),
    Mean = mean(Minutes),
    Median = median(Minutes),
    Max = max(Minutes),
    Min = min(Minutes),
    Count = n(),
    .groups = "drop"  # Use .groups argument to drop the grouping
  )
```
### Inspect the new table that has been created

```{r}
colnames(bikesrides2)  #List of column names
nrow(bikesrides2)  #How many rows are in data frame?
dim(bikesrides2)  #Dimensions of the data frame?
head(bikesrides2)  #See the first 6 rows of data frame.  Also tail(bikesrides2)
str(bikesrides2)  #See list of columns and data types (numeric, character, etc)
summary(bikesrides2)  #Statistical summary of data. Mainly for numeric
```
### Calculate moving average

```{r}
bikesrides2$CntMA <- forecast::ma(bikesrides2$Count,28)
```
## Plot of Rides By Date
#### Summary Stats: Counts

* Summary of Hourly Counts

### Summary of Hourly Counts

```{r}
summary(bikesrides2$Count)
```

* Count of rides by Hour

### Table of Counts by Hour

```{r}
xtabs(bikesrides2$Count~bikesrides2$start_hour)
```

### Add Monthly coloumn

```{r}
bikesrides2$Monthly <- lubridate::month(bikesrides2$Weekly)
```

### Create a visualization for"Count of Rides per Day"

```{r}
bikesrides2 %>% ggplot() + geom_col(aes(x=Weekly,y=Count)) +
  scale_y_continuous(labels = comma) +
  labs(title = "Count of Rides per Day",
       subtitle = "Bases on 28 day moving average",
       y="Average rides per day")  
```
### Create a visualization for"Count of Rides by Hours"

```{r}
bikesrides2 %>% ggplot() + geom_col(aes(x=start_hour,y=Count)) +
  scale_y_continuous(labels = comma) +
  labs(title = "Count of Rides by Hours",
       y="Rides per Hour") 

```

## Count of Rides by Bike Type
#### Summary of Bike Types

```{r}
bikestype <- bike_rides %>% 
  group_by(member_casual, rideable_type, Weekly = floor_date(started_at, "week")) %>%
  summarise(
    Minutes = sum(Minutes),
    Mean = mean(Minutes),
    Median = median(Minutes),
    Max = max(Minutes),
    Min = min(Minutes),
    Count = n(),
    .groups = "drop"  # Use .groups argument to drop the grouping
  )

```

* Count by Bike Type(Total by Week)

### Create a visualization for "Count of Rides by Bike Type"

```{r}
ggplot(bikestype) + geom_area(aes(x=Weekly,y=Count,col=rideable_type)) +
  scale_y_continuous(labels = comma) +
  labs(title="Count of Rides by Bike Type")
```
### Create a visualization for "Top 20 Start Stations by Ride Count"

```{r}
bike_rides %>% count(start_station_name, sort = TRUE) %>%
  top_n(20) %>% ggplot() + geom_col(aes(x=reorder(start_station_name,n),y=n)) +
  coord_flip() + labs(title = "Top 20 Start Stations by Ride Count",
                      y = "Count of Rides",x="Station Name") +
  scale_y_continuous(labels = comma)
```

### Create a visualization for "Top 20 Start Stations by Ride Count" with member_casual variable

```{r}
bike_rides %>%
  count(start_station_name, member_casual, sort = TRUE) %>%
  top_n(20) %>%
  ggplot() + 
  geom_col(aes(x = reorder(start_station_name, n), y = n, fill = member_casual)) +
  coord_flip() + 
  labs(title = "Top 20 Start Stations by Ride Count",
       y = "Count of Rides", x = "Station Name") +
  scale_y_continuous(labels = comma)
```

### Create a visualization for "Count of Rides by Rider Type"

```{r}
ggplot(bikestype) + geom_col(aes(x=Weekly,y=Count,fill=member_casual)) +
  scale_y_continuous(labels = comma) +
  labs(title="Count of Rides by Rider Type")
```

### Create a visualization for "Total Ride Minutes by Week"

```{r}
ggplot(bikestype) + geom_col(aes(x=Weekly,y=Minutes)) +
  scale_y_continuous(labels = comma) + facet_wrap(~rideable_type) +
  labs(title="Total Ride Minutes by Week")
```
### Create a visualization for "Rides Minutes by Bike Type and Week"

```{r}
ggplot(bikestype,aes(x=Weekly,y=Minutes,fill=rideable_type)) + 
  geom_area(stat = "identity", position = position_dodge(), alpha = 0.75) +
  scale_y_continuous(labels = comma) + 
  labs(title = "Rides Minutes by Bike Type and Week",
       y="Bike Trip In Minutes")
```

## Begin Analysis Of Minutes of Ride Time

### Create a visualization for "Average Trip Minutes by Week"

```{r}
ggplot(bikesrides2) + geom_col(aes(x=Weekly,y=Mean)) +
  labs(title="Average Trip Minutes by Week", y ="Average Ride Time(minutes)") +
  scale_y_continuous(labels = comma)
```
### Create visualize the number of rides by rider type

```{r}
bike_rides %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(
    number_of_rides = n(),
    average_duration = mean(Minutes)
  ) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = position_dodge()) +
  scale_y_continuous(labels = scales::comma) +
  labs(
    title = "Total Annual Rides by Weekday",
    x = "Weekday",
    y = "Number of Rides",
    fill = "Member Type"
  )
```

### Create visualize for average duration

```{r}
bike_rides %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday, .groups = "drop") %>%
  summarise(
    number_of_rides = n(),
    average_duration = mean(Minutes)
  ) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = average_duration / 60, fill = member_casual)) +
  geom_col(position = position_dodge()) +
  scale_y_continuous(labels = scales::comma, name = "Average Duration (hours)") +
  labs(
    title = "Average Annual Ride Duration by Weekday",
    x = "Weekday",
    fill = "Member Type"
  )
```
