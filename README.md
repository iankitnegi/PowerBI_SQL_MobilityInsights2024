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
Business Request- 1: City-Level Fare and Trip Summary Report
Generate a report that displays the total trips, average fare per km, average fare per trip, and the percentage contribution of each city's trips to the overall trips. This report will help in assessing trip volume, pricing efficiency, and each city's contribution to the overall trip count.  

Fields:
- city name
- totaI_trips
- avg_fare_per_km
- avg fare per trip
- %_contribution_to_totaI_trips
________________________________
Business Request-2: MonthIyCity-Level Trips Target Performance Report  
Generate a report that evaluates the target performance fortrips at the monthly and city level. For each city and month, compare theactual total trips with the target trips and categorise the performance as follows:
 • Ifactual trips are greater than target trips, mark it as "Above Target".
 • Ifactual trips are less than orequal to target trips, mark it as "Below Target".
 Additionally, calculate the% difference between actual and target trips to quantify the
 performance gap.
 Fields:
 • City_name
 • month name
 • actual trips
 • target_trips
 • performance status
 • % differenc
