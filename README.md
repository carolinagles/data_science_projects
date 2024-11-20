# Analytical Study of Taxi Rides and Weather Impact in Chicago - Data Collection and Storage (SQL)

This project aims to analyze patterns in taxi ride data and assess the impact of weather conditions on ride durations in Chicago. The study focuses on understanding passenger preferences, evaluating external factors like weather, and testing a hypothesis about weather's influence on ride durations.

### Datasets Used (SQL)
**Database Tables**:
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

### CSV Files Created:
1. **`project_sql_result_01.csv`**  
   - Contains taxi companies and their number of rides on November 15-16, 2017.

2. **`project_sql_result_04.csv`**  
   - Lists Chicago neighborhoods with average trip completions in November 2017.

3. **`project_sql_result_07.csv`**  
   - Includes data about trips from Loop to O'Hare International Airport, including weather conditions and trip durations.

### Methodology
**SQL Tasks**:
1. Data extraction was performed to analyze:  
   - The number of rides for each taxi company between November 15-16, 2017.  
   - Rides for companies containing "Yellow" or "Blue" in their names between November 1-7, 2017.  
   - Ride counts for the two most popular companies (`Flash Cab` and `Taxi Affiliation Services`) and comparison with other companies.

2. The relationship between trips and weather conditions was explored using `JOIN` operations on `start_ts` (trip start time) and `ts` (weather record timestamp).

3. A hypothesis was tested regarding the impact of rainy Saturdays on ride durations between Loop and O'Hare.

**Python Tasks**:
1. Data cleaning and exploration were conducted on extracted datasets.
2. Visualizations were created:  
   - A bar chart for taxi companies and their number of rides.  
   - A graph of the top 10 neighborhoods by trip completions.
3. A hypothesis test was performed to analyze average ride durations on rainy Saturdays.

### Hypothesis Testing  
- **Null Hypothesis (H₀)**: The average trip duration from Loop to O'Hare does not change on rainy Saturdays.  
- **Alternative Hypothesis (H₁)**: The average trip duration from Loop to O'Hare increases on rainy Saturdays.  
- A significance level (`α`) was selected, and appropriate statistical tests were applied to validate the hypothesis.

### Conclusions

In conclusion, the findings of the taxi trip analysis have been presented, highlighting the relationship between weather conditions and trip characteristics. The data has been examined to identify patterns related to specific locations, day of the week, and weather. Based on these insights, recommendations for improving operational efficiency and customer experience have been suggested. The analysis has been thoroughly reviewed, and future actions have been outlined to further optimize taxi services and address identified trends. 

# Chicago Weather Data Extraction - Web Scrapping
This script demonstrates how to extract tabular weather data from a web page using `requests` and `BeautifulSoup`, then organize it into a structured format using `pandas`. The extracted dataset contains weather records for Chicago in 2017.

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
When the script is executed, a `pandas` DataFrame containing the weather data is displayed. The columns correspond to the table headers, and each row represents a day's weather record.

Example DataFrame Output

```
       Date  High Temp (°F)  Low Temp (°F)  Precipitation (in)
0  01/01/17             38             25                 0.00
1  01/02/17             40             26                 0.02
2  01/03/17             45             28                 0.00
...
```

- The table is identified by its `id="weather_records"` attribute in the HTML structure.
- `BeautifulSoup` is configured to parse the HTML using the `lxml` parser.
- Data cleaning or transformation (e.g., converting numeric strings to floats) can be performed after the DataFrame creation if necessary.

# SQL Queries

### 1. SQL Query: Number of Trips by Cab Company

This query calculates the number of trips completed by each cab company within a specified date range. The goal is to identify which companies had the highest trip counts over the specified period.

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

**Key Components**

- **Tables**:
  - `cabs`: Stores information about cab companies.
  - `trips`: Contains trip details, including start times and the associated cab IDs.
  
- **Logic**:
  - Joins the `cabs` and `trips` tables based on `cab_id`.
  - Filters trips occurring between **November 15, 2017** and **November 16, 2017**.
  - Groups results by cab company name.
  - Counts the number of trips (`trips_amount`) for each company.
  - Sorts companies by the highest trip count.

**Output**:
- A table showing cab companies and their respective trip counts, ordered from the highest to the lowest.

---

### 2. SQL Query: Trips Analysis for "Yellow" and "Blue" Cab Companies

This query counts the number of trips completed by cab companies with names containing **"Yellow"** or **"Blue"** over a specified date range. The goal is to identify and compare the performance of these two groups.

```sql
SELECT
    cabs.company_name AS company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM 
    cabs
INNER JOIN 
    trips 
ON 
    trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
    AND cabs.company_name LIKE '%Yellow%'
GROUP BY company_name
UNION ALL
SELECT
    cabs.company_name AS company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM 
    cabs
INNER JOIN 
    trips 
ON 
    trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
    AND cabs.company_name LIKE '%Blue%'
GROUP BY company_name;
```

**Key Components**

- **Filters**:
  - Includes cab companies whose names contain **"Yellow"** or **"Blue"**.
  - Limits data to trips within the date range of **November 1, 2017** to **November 7, 2017**.

- **Grouping**:
  - Groups the trip counts by each company’s name.

- **Union**:
  - Combines results from "Yellow" and "Blue" cab companies into a single output.

**Output Columns**:
- `company_name`: Name of the cab company.
- `trips_amount`: Total number of trips completed by that company during the specified time.

---

### 3. SQL Query: Categorizing Taxi Companies into Groups

This SQL query categorizes taxi companies into three groups (`Flash Cab`, `Taxi Affiliation Services`, and `Other`) based on their `company_name` and calculates the number of trips (`trips_amount`) completed by each group within the specified date range. The results are ordered by the total number of trips in descending order.

```sql
SELECT
    CASE 
        WHEN company_name = 'Flash Cab' THEN 'Flash Cab' 
        WHEN company_name = 'Taxi Affiliation Services' THEN 'Taxi Affiliation Services' 
        ELSE 'Other' 
    END AS company,
    COUNT(trips.trip_id) AS trips_amount                
FROM 
    cabs
INNER JOIN 
    trips 
ON 
    trips.cab_id = cabs.cab_id
WHERE 
    CAST(trips.start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY 
    company
ORDER BY 
    trips_amount DESC;
```

**Key Components**

- **`CASE` Statement**:
  - Classifies the taxi companies into three groups:
    - `Flash Cab`: Matches exactly to "Flash Cab."
    - `Taxi Affiliation Services`: Matches exactly to "Taxi Affiliation Services."
    - `Other`: All other company names are grouped under "Other."

- **`COUNT(trips.trip_id)`**:
  - Calculates the total number of trips (`trips.trip_id`) for each company group.

- **`INNER JOIN`**:
  - Links the `cabs` table to the `trips` table using the `cab_id` column to ensure only relevant records are considered.

- **`WHERE` Clause**:
  - Filters trips where the start timestamp (`start_ts`) falls between **November 1, 2017** and **November 7, 2017**.
  - Uses `CAST(trips.start_ts AS date)` to extract the date portion from the timestamp.

- **`GROUP BY` Clause**:
  - Groups the aggregated trip counts by the newly created `company` column.

- **`ORDER BY` Clause**:
  - Sorts the results by the number of trips (`trips_amount`) in descending order, showing the most popular group first.

---

### 4. SQL Query: Extracting Specific Neighborhoods

This SQL query retrieves the `neighborhood_id` and `name` of neighborhoods whose names either contain the substring "Hare" (e.g., "O'Hare") or match exactly "Loop." It uses the `LIKE` operator to perform a pattern match.

```sql
SELECT
    neighborhood_id,
    name
FROM 
    neighborhoods
WHERE 
    name LIKE '%Hare' OR name LIKE 'Loop';
```

**Key Components**

- **`SELECT` Clause**:
  - Retrieves:
    - `neighborhood_id`: The unique identifier for each neighborhood.
    - `name`: The name of the neighborhood.

- **`WHERE` Clause**:
  - Filters the records based on the `name` column using the `LIKE` operator:
    - `name LIKE '%Hare'`: Matches neighborhood names that **end with "Hare"**, such as "O'Hare."
    - `name LIKE 'Loop'`: Matches names that are **exactly "Loop"**.

- **`OR` Logical Operator**:
  - Combines the two conditions, ensuring neighborhoods that satisfy either of the criteria are included in the result.

---

### 5. SQL Query: Categorizing Weather Conditions

This SQL query categorizes weather conditions as either "Bad" or "Good" based on the presence of specific keywords (`rain` or `storm`) in the weather descriptions. It uses the `CASE` statement for conditional logic.

```sql
SELECT
    ts,
    CASE
        WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
        ELSE 'Good'
    END AS weather_conditions
FROM 
    weather_records;
```

**Key Components**

- **`SELECT` Clause**:
  - Retrieves:
    - `ts`: The timestamp column from the `weather_records` table, representing the date and time of the weather record.
    - A calculated column (`weather_conditions`) using the `CASE` statement to classify weather conditions.

- **`CASE` Statement**:
  - Performs conditional checks on the `description` column:
    - `description LIKE '%rain%'`: Matches descriptions containing the word "rain."
    - `description LIKE '%storm%'`: Matches descriptions containing the word "storm."
  - If either condition is true, the `weather_conditions` column is set to 'Bad'; otherwise, it is set to 'Good'.

- **`LIKE` Operator**:
  - Used for pattern matching to detect keywords within the weather descriptions.

---

### 6. SQL Query: Taxi Trip Analysis with Weather Conditions

This query analyzes taxi trips under specific conditions and joins weather data to assess its relationship with trip characteristics.

```sql
SELECT
    start_ts,
    T.weather_conditions,
    duration_seconds
FROM 
    trips
INNER JOIN (
    SELECT
        ts,
        CASE
            WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
            ELSE 'Good'
        END AS weather_conditions
    FROM 
        weather_records          
) T ON T.ts = trips.start_ts
WHERE 
    pickup_location_id = 50 
    AND dropoff_location_id = 63 
    AND EXTRACT(DOW FROM trips.start_ts) = 6
ORDER BY 
    trip_id;
```

**Key Components**

- **Subquery (`T`)**:
  - Extracts `ts` (timestamp) and categorizes weather conditions as "Bad" (rain or storm in `description`) or "Good" (otherwise) using a `CASE` statement.
  - Represents weather data for each timestamp.

- **`INNER JOIN`**:
  - Joins the `trips` table with the subquery `T` on matching timestamps (`T.ts = trips.start_ts`).
  - Ensures that only trips occurring at the same time as recorded weather conditions are included.

- **`SELECT` Clause**:
  - Retrieves:
    - `start_ts`: The timestamp when the trip began.
    - `T.weather_conditions`: The weather
    
---

# The analysis is continued using Python.