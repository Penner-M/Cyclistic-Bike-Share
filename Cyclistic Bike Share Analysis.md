# Cyclistic-Bike-Share
Cyclistic Bike Share Analysis
Michelle Penner
2023-08-05
Summary of Analysis for Cyclistic Bike Share
Annually, members ride more frequently than casual users. This suggests that members are more regular and frequent users of the bike-sharing service compared to casual users.

Tuesday to Thursday are the most popular days for members, indicating a regular weekday commuting pattern. This aligns with the idea that members use the service primarily for daily commuting purposes during the workweek.

Both members and casual users ride most often between 3-6 p.m., indicating high demand during the late afternoon hours. This peak in usage could be attributed to people using the bikes for leisure or running errands in the late afternoon.

The top start and end stations for members are Kingsbury and Kinzie St., while casual users prefer Streeter Dr. and Grand Ave. This difference in station preferences may be related to the locations of residential and commercial areas frequented by members and casual users.

Casual users predominantly ride on Saturday and Sunday, indicating a preference for weekends. This suggests that casual users use the service more for leisure or recreational activities during the weekends.

Casual users ride longer distances on average compared to members. This difference in ride distances might imply that casual users prefer longer rides, exploring various destinations, or using the service for leisurely city tours.

July records the highest ridership, while December has the lowest. This seasonal variation might be due to weather conditions, as more people tend to use bike-sharing services during pleasant weather.

Saturday is the most popular day of the week across all users and is the only day casual users slightly surpass members in usage. This indicates that Saturdays are the busiest days for both groups, and casual users might have a slightly higher usage on Saturdays compared to other weekdays.

My 3 Recommendations Based On My Analysis
The marketing strategy should focus on increasing membership awareness and attracting casual users by offering special deals in locations where casual users are most frequent. By targeting specific areas, the marketing efforts can be more effective while reducing overall expenditure.

Incentives should be designed to encourage casual riders to transition to becoming members. Emphasize the benefits of membership, such as reducing one carbon footprint, saving money, and improving health through low-impact physical activity. These angles can resonate with potential members and motivate them to make the switch.

Conduct targeted surveys for all user types to gain valuable insights. By using data-driven user type hotspots, the surveys can gather detailed feedback from different user segments. This information will help in understanding user preferences, needs, and pain points, allowing for more informed decision-making and tailored marketing strategies.

How do annual members and casual riders use Cyclistic differently?
The provided information pertains specifically to the difference between annual members and casual riders during the period from July 2022 to June 2023. It is important to note that these insights are limited to the data we have on hand, and further implications may require addressing the remaining stakeholder questions.

Here is a summary of the key findings:

Members prefer riding on weekdays, with the most frequent rides occurring from Tuesday to Thursday. In contrast, casual users prefer weekends, particularly Saturdays, suggesting different usage patterns for both groups. Kingsbury and Kinzie St. are the top start and end stations for members, whereas Streeter Dr. and Grand Ave. are preferred by casual users. Station preferences may align with the locations of residential and commercial areas frequented by each group. Classic bikes are the most preferred bike type among both members and casual users, indicating a preference for traditional biking options. Both groups commonly ride between 3-6 p.m., likely corresponding to the late afternoon when many people use the service for various activities. April to October is the preferred period for both members and casual users, likely due to favorable weather conditions during these months. July is the most popular month, likely due to summer weather and increased outdoor activities. December records the lowest ridership, possibly due to colder weather and holiday-related factors. Saturday is the most popular day of the week across all users, and it is the only day when casual users slightly surpass members in usage, although the difference is marginal. It is important to consider these findings when formulating strategies and making data-driven decisions to better serve both member and casual user segments effectively.

Data Wrangling Change Log
The data has been made available by Motivate International Inc. under this License.

Date Range: 2022-07-01 to 2023-06-30
August 5, 2023:

Total Rows: 5,779,444

I raised the following concern (about the data) with Lily Moreno(The director of marketing): 858,403 rows with missing Starting Station names (and ID),

I was advised by Lily Moreno that we could ignore rows with trip duraton <=0. We could also ignore rows with missing start_station_name, and also ignore stations_id.

And should focus on doing using the start_station_name to preform aggregate functions on date, start_station_name, member_casual and rideable_type.

Cleaning reducted rows to 4,921,041
