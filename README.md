# Problem Statement  
Goodcabs, a cab service company, has quickly established itself as a prominent player in the Indian transportation market by focusing on tier-2 cities. Committed to supporting local drivers and enhancing passenger experiences. Goodcabs aims to achieve ambitious performance targets for 2024. To meet these objectives, the management team is keen on analyzing operational performance across key metrics such as:

- Trip volume trends.
- Passenger satisfaction levels.
- Repeat passenger rate.
- Trip distribution across cities.
- Balance between new and repeat passengers.

However, with limited resources and urgent requirements, the task of delivering actionable insights has been assigned to the data team. The challenge lies in aggregating, analyzing, and visualizing critical data to provide the Chief of Operations with a clear understanding of performance trends and areas for improvement. 

This repository aims to serve as a centralized hub for developing analytics tools, generating insights, and tracking progress toward Goodcabs' 2024 targets, ensuring sustainable growth and improved passenger satisfaction.

## 1. ASK  
COO: Bruce Haryali  

### Questions:
*Business Request - 1: City-Level Fare and Trip Summary Report*  
Generate a report that displays the total trips, average fare per km, average fare per trip, and the percentage contribution of each city's trips to the overall trips. This report will help in assessing trip volume, pricing efficiency, and each city's contribution to the overall trip count.  

Fields: 
- city name
- totaI_trips
- avg_fare_per_km
- avg fare per trip
- %_contribution_to_totaI_trips


*Business Request - 2: Monthly City-Level Trips Target Performance Report*  
Generate a report that evaluates the target performance for trips at the monthly and city level. For each city and month, compare the actual total trips with the target trips and categorise the performance as follows:  
- If actual trips are greater than target trips, mark it as "Above Target".
- If actual trips are less than or equal to target trips, mark it as "Below Target".
Additionally, calculate the % difference between actual and target trips to quantify the performance gap.

Fields:
- City_name
- month name
- actual trips
- target_trips
- performance status
- % difference


*Business Request - 3: City-Level Repeat Passenger Trip Frequency Report*   
Generate a report that shows the percentage distribution of repeat passengers by the number of trips they have taken in each city. Calculate the percentage of repeat passengers who took 2 trips, 3 trips, and so on, up to 10 trips.  
Each column should represent a trip count category, displaying the percentage of repeat passengers who fall into that category out of the total repeat passengers for that city.  
This report will help identify cities with high repeat trip frequency, which can indicate strong customer loyalty or frequent usage patterns. 

Fields: 
- city name
- 2-Trips
- 3-Trips
- 4-Trips
- 5-Trips
- 6-Trips
- 7-Trips
- 8-Trips
- 9-Trips
- 10-Trips 


*Business Request - 4: Identify Cities with Highest and Lowest Total New Passengers*  
Generate a report that calculates the total new passengers for each city and ranks them based on this value. Identify the top 3 cities with the highest number of new passengers as well as the bottom 3 cities with the lowest number of new passengers, categorising them as "Top 3" or "Bottom 3" accordingly. 

Fields:
- city name
- totaI_new_passengers
- city_category ("Top 3" or "Bottom 3")


*Business Request - 5: Identify Month with Highest Revenue for Each City*   
Generate a report that identifies the month with the highest revenue for each city. For each city, display the month name, the revenue amount for that month, and the percentage contribution of that month's revenue to the city's total revenue.  

Fields
- city_name
- highest revenue month
- revenue
- percentage contribution (%)


*Business Request - 6: Repeat Passenger Rate Analysis*  
Generate a report that calculates two metrics:
1. Monthly Repeat Passenger Rate: Calculate the repeat passenger rate for each city and month by comparing the number of repeat passengers to the total passengers.  
2.	City-wide Repeat Passenger Rate: Calculate the overall repeat passenger rate for each city, considering all passengers across months.  

These metrics will provide insights into monthly repeat trends as well as the overall repeat behaviour for each city.      

Fields:   
- city name
- month
- totaI_passengers
- repeat_passengers
- monthly repeat	assenger rate (%): Repeat passenger rate at the city and month level
- city repeat_passenger rate (%): Overall repeat passenger rate for each city, aggregated across months




This document provides metadata descriptions for the tables in the `trips_db` and `targets_db` databases. 




trips_db: This database contains both detailed and aggregated data on trips, passenger types, and repeat trip behavior for Goodcabs' operations across tier-2 cities. It organizes trip data by city, month, and day type (weekday or weekend), enabling comprehensive analysis of travel patterns, passenger demographics, and repeat usage trends.      

1. dim_city
Purpose: This table provides city-specific details, enabling location-based analysis of trips and passenger behavior across Goodcabs’ operational areas.
- city_id: A unique identifier for each city, using a standardized code (e.g., RJ01 for Jaipur).
- city_name: The name of the city where Goodcabs operates (e.g., Jaipur, Lucknow), used for location-based grouping and insights.

SELECT
	c.city_name, 
	COUNT(*) AS total_trips,
	ROUND(SUM(t.fare_amount)/SUM(t.distance_travelled_km),2) AS avg_fare_per_km,
    ROUND(SUM(t.fare_amount)/COUNT(*), 2) AS avg_fare_per_trip,
    ROUND(COUNT(*)/(SELECT COUNT(*) FROM fact_trips)*100, 2) AS pct_contributtion
FROM dim_city c
JOIN fact_trips t ON c.city_id=t.city_id
GROUP BY city_name
ORDER BY 5 DESC;

2. dim_date
Purpose: This table provides date-specific details that enable time-based grouping and analysis, helping to identify patterns across days, months, and weekends versus weekdays.

- date: The specific date for each entry (formatted as YYYY-MM-DD), used for daily-level analysis and to join with trip data.
- start_of_month: The first day of the month to which the date belongs, useful for grouping data by month in analyses.
- month_name: The name of the month (e.g., January, February) corresponding to the date, helpful for month-based aggregations.
- day_type: Indicator of whether the date is a weekday or weekend, enabling analysis of demand and trip patterns across different day types.



3. fact_passenger_summary (Aggregated Data)
Purpose: This table provides an aggregated summary of passenger counts for each city by month. It includes data on total passengers, new passengers, and repeat passengers, giving a high-level overview of passenger distribution patterns across different locations and times.

- month: The start date of the month (formatted as YYYY-MM-DD). All metrics in this table are aggregated for the entire month, aligning with other time-based analyses.
- city_id: Unique identifier for the city.
- total_passengers: The aggregated count of all passengers (both new and repeat) for the specified city and month.
- new_passengers: The count of passengers taking their first ride in the specified city and month.
- repeat_passengers: The count of passengers who have taken at least one ride in previous months and returned to ride again in the specified city and month.



4. dim_repeat_trip_distribution (Aggregated Data)
Purpose: This table provides a breakdown of repeat trip behavior, aggregated by month and city. It details how many times repeat passengers rode within the given month, categorized by trip frequency. To keep things simple and ensure uniformity across cities, we have included trip frequencies up to a maximum of 10 trips per month. This allows for an analysis of repeat trip patterns at a granular level.

- month: The start date of the month (formatted as YYYY-MM-DD). All metrics in this table represent aggregated values for the entire month, allowing alignment with other date-based analyses.
- city_id: Unique identifier for the city.
- trip_count: The number of trips taken by repeat passengers within the specified city and month (e.g., "3-Trips" for passengers who took 3 trips).
- repeat_passenger_count: The count of repeat passengers associated with each specified trip_count for the city and month.



5. fact_trips
Purpose: This table provides detailed information on each individual trip, including trip-specific metrics like distance, fare, and ratings, which can be used for granular trip-level analysis.

- trip_id: Unique identifier for each trip.
- date: The exact date of the trip (formatted as YYYY-MM-DD).
- city_id: Unique identifier for the city where the trip took place.
- passenger_type: Indicates if the passenger is new (first-time rider) or repeat (returning rider).
- distance_travelled (km): Total distance covered in the trip, measured in kilometers.
- fare_amount: Total fare amount paid by the passenger for the trip, in local currency.
- passenger_rating: Rating provided by the passenger for the trip, on a scale from 1 to 10.
- driver_rating: Rating provided by the driver for the passenger, on a scale from 1 to 10.
------------------------------------




------------------------------------
targets_db: This database holds Goodcabs' monthly targets for each city, including goals for trip counts, new passenger acquisition, and average passenger ratings. It enables performance evaluations against key benchmarks set by the company.



1. city_target_passenger_rating
- city_id: Unique identifier for each city.
- target_avg_passenger_rating: The target average passenger rating that Goodcabs aims to achieve for each city, used to monitor customer satisfaction.


2. monthly_target_new_passengers
- month: The start date of the target month (formatted as YYYY-MM-DD) to align with other time-based data.
- city_id: Unique identifier for each city.
- target_new_passengers: The target number of new passengers Goodcabs aims to acquire in each city for the specified month, supporting growth tracking.


3. monthly_target_trips
- month: The start date of the target month (formatted as YYYY-MM-DD) for easy alignment with monthly performance data.
- city_id: Unique identifier for each city.
- total_target_trips: The target number of total trips Goodcabs aims to achieve for each city and month, enabling assessment of trip volume performance.
------------------------------------















