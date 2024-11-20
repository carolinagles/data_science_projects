
# Analytical Study of Taxi Rides and Weather Impact in Chicago

## Overview
This project was undertaken to analyze patterns in taxi ride data and assess the impact of weather conditions on ride durations in Chicago. 

## Project Description
The project was designed with the following goals:  
- Understanding passenger preferences.  
- Evaluating external factors (e.g., weather) affecting taxi rides.  
- Testing a hypothesis about weather impact on ride durations.  

A database containing detailed information about taxi rides, neighborhoods, and weather records was utilized. The study was divided into SQL and Python tasks.

---

## Datasets Used
### Database Tables:
1. **`neighborhoods`**  
   - `name`: Neighborhood name.  
   - `neighborhood_id`: Unique identifier for the neighborhood.

2. **`cabs`**  
   - `cab_id`: Vehicle identifier.  
   - `vehicle_id`: Technical vehicle ID.  
   - `company_name`: Taxi company name.

3. **`trips`**  
   - `trip_id`: Unique identifier for each trip.  
   - `cab_id`: Identifier of the vehicle operating the trip.  
   - `start_ts`: Start date and time (rounded to the hour).  
   - `end_ts`: End date and time (rounded to the hour).  
   - `duration_seconds`: Duration in seconds.  
   - `distance_miles`: Distance covered in miles.  
   - `pickup_location_id`: Neighborhood ID where the trip started.  
   - `dropoff_location_id`: Neighborhood ID where the trip ended.

4. **`weather_records`**  
   - `record_id`: Weather record identifier.  
   - `ts`: Date and time of the record.  
   - `temperature`: Temperature recorded.  
   - `description`: Brief description of weather conditions (e.g., "light rain," "scattered clouds").

### CSV Files:
1. **`project_sql_result_01.csv`**  
   - Contains taxi companies and their number of rides on November 15-16, 2017.

2. **`project_sql_result_04.csv`**  
   - Lists Chicago neighborhoods with average trip completions in November 2017.

3. **`project_sql_result_07.csv`**  
   - Includes data about trips from Loop to O'Hare International Airport, including weather conditions and trip durations.

---

## Methodology
### SQL Tasks:
1. Data extraction was performed to analyze:  
   - The number of rides for each taxi company between November 15-16, 2017.  
   - Rides for companies containing "Yellow" or "Blue" in their names between November 1-7, 2017.  
   - Ride counts for the two most popular companies (`Flash Cab` and `Taxi Affiliation Services`) and comparison with other companies.

2. The relationship between trips and weather conditions was explored using `JOIN` operations on `start_ts` (trip start time) and `ts` (weather record timestamp).

3. A hypothesis was tested regarding the impact of rainy Saturdays on ride durations between Loop and O'Hare.

### Python Tasks:
1. Data cleaning and exploration were conducted on extracted datasets.
2. Visualizations were created:  
   - A bar chart for taxi companies and their number of rides.  
   - A graph of the top 10 neighborhoods by trip completions.
3. A hypothesis test was performed to analyze average ride durations on rainy Saturdays.

---

## Hypothesis Testing
- **Null Hypothesis (H₀)**: The average trip duration from Loop to O'Hare does not change on rainy Saturdays.  
- **Alternative Hypothesis (H₁)**: The average trip duration from Loop to O'Hare increases on rainy Saturdays.  
- A significance level (`α`) was selected, and appropriate statistical tests were applied to validate the hypothesis.

---

## Evaluation Criteria
The following aspects were considered:
- Accurate data retrieval and processing.
- Appropriate grouping, slicing, and joining of data.
- Proper hypothesis formulation and testing.
- Clear visualizations and well-documented insights.

---

## Results and Conclusion
The findings will be detailed in the project report, with conclusions based on the data analysis and hypothesis testing.

---

This README documents the project and ensures its clarity for further review.