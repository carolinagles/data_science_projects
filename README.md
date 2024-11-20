
# Analytical Study of Taxi Rides and Weather Impact in Chicago

## Overview
This project was undertaken to analyze patterns in taxi ride data and assess the impact of weather conditions on ride durations in Chicago. 

## Project Description
The project was designed with the following goals:  
- Understanding passenger preferences.  
- Evaluating external factors (e.g., weather) affecting taxi rides.  
- Testing a hypothesis about weather impact on ride durations.  

A database containing detailed information about taxi rides, neighborhoods, and weather records was utilized. The study was divided into SQL and Python tasks.

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

## Hypothesis Testing
- **Null Hypothesis (H₀)**: The average trip duration from Loop to O'Hare does not change on rainy Saturdays.  
- **Alternative Hypothesis (H₁)**: The average trip duration from Loop to O'Hare increases on rainy Saturdays.  
- A significance level (`α`) was selected, and appropriate statistical tests were applied to validate the hypothesis.



# Chicago Weather Data Extraction Project - Web Scrapping

## Objective

This script demonstrates how to extract tabular weather data from a web page using `requests` and `BeautifulSoup`, then organize it into a structured format using `pandas`. The extracted dataset contains weather records for Chicago in 2017.

## Libraries Used

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

## Script Description

1. **Fetching the Web Page**:
   - The script uses the `requests` library to fetch an HTML page containing weather data.

2. **Parsing the HTML**:
   - The `BeautifulSoup` library is used to parse the HTML page and locate the table of interest (identified by its `id="weather_records"`).

3. **Extracting Table Data**:
   - Table headers are extracted from the `<th>` elements.
   - Table rows are extracted from the `<td>` elements.

4. **Creating a DataFrame**:
   - The data is organized into a `pandas` DataFrame, where column names are derived from the table headers.

## Code Example

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL of the weather data page
URL = 'https://practicum-content.s3.us-west-1.amazonaws.com/data-analyst-eng/moved_chicago_weather_2017.html'

# Fetch and parse the page
req = requests.get(URL)
soup = BeautifulSoup(req.text, 'lxml')

# Find the table containing weather records
table = soup.find('table', attrs={"id": "weather_records"})

# Extract table headers
heading_table = []
for row in table.find_all('th'):
    heading_table.append(row.text)

# Extract table content
content = []
for row in table.find_all('tr'):
    if not row.find_all('th'):
        content.append([element.text for element in row.find_all('td')])

# Create a pandas DataFrame
weather_records = pd.DataFrame(content, columns=heading_table)

# Print the DataFrame
print(weather_records)
```
## Expected Output

When the script is executed, a `pandas` DataFrame containing the weather data is displayed. The columns correspond to the table headers, and each row represents a day's weather record.

### Example DataFrame Output

```
       Date  High Temp (°F)  Low Temp (°F)  Precipitation (in)
0  01/01/17             38             25                 0.00
1  01/02/17             40             26                 0.02
2  01/03/17             45             28                 0.00
...
```

## Notes

- The table is identified by its `id="weather_records"` attribute in the HTML structure.
- `BeautifulSoup` is configured to parse the HTML using the `lxml` parser.
- Data cleaning or transformation (e.g., converting numeric strings to floats) can be performed after the DataFrame creation if necessary.

#  SQL Query

This SQL query retrieves the number of trips completed by each cab company between **November 15, 2017**, and **November 16, 2017**. Below is a breakdown of its functionality:

---

#### Query Breakdown:s

1. **FROM Clause**:
   - Tables: 
     - `cabs`: Contains information about cab companies.
     - `trips`: Contains information about individual trips, including their start times and the associated cab IDs.

2. **INNER JOIN**:
   - The `INNER JOIN` combines data from the `cabs` table and the `trips` table using the common column `cab_id`. This ensures only matching records between the two tables are included.

3. **WHERE Clause**:
   - Filters the data to include only trips where the start timestamp (`start_ts`) falls between **November 15, 2017**, and **November 16, 2017**. The `CAST(trips.start_ts AS date)` function extracts only the date portion of the timestamp for comparison.

4. **SELECT Clause**:
   - `cabs.company_name`: Retrieves the name of the cab company.
   - `COUNT(trips.trip_id) AS trips_amount`: Counts the number of trips (`trip_id`) for each company.

5. **GROUP BY Clause**:
   - Groups the data by `company_name` so that the count of trips is calculated for each company.

6. **ORDER BY Clause**:
   - Orders the results in descending order of the trip count (`trips_amount`), showing companies with the most trips at the top.



```markdown
## SQL Query: Number of Trips by Cab Company

This query calculates the number of trips completed by each cab company within a specified date range. The goal is to identify which companies had the highest trip counts over the specified period.

### Query

```sql
SELECT
    cabs.company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM 
    cabs
    INNER JOIN 
    trips 
    ON 
    trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS date) BETWEEN '2017-11-15' AND '2017-11-16'
GROUP BY 
    company_name
ORDER BY 
    trips_amount DESC;
```

### Key Components

1. **Tables**:
   - `cabs`: Stores information about cab companies.
   - `trips`: Contains trip details, including start times and the associated cab IDs.

2. **Logic**:
   - Joins the `cabs` and `trips` tables based on `cab_id`.
   - Filters trips occurring between **November 15, 2017**, and **November 16, 2017**.
   - Groups results by cab company name.
   - Counts the number of trips (`trips_amount`) for each company.
   - Sorts companies by the highest trip count.

3. **Output**:
   - A table showing cab companies and their respective trip counts, ordered from the highest to the lowest number of trips.

### Sample Output

| company_name      | trips_amount |
|-------------------|--------------|
| Yellow Cab        | 200          |
| Green Cab         | 150          |
| Chicago Cabs LLC  | 100          |

```

---