
# Analytical Study of Taxi Rides and Weather Impact in Chicago - Data Collection and Storage (SQL)

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


# 1. Chicago Weather Data Extraction Project - Web Scrapping

**Objective**

This script demonstrates how to extract tabular weather data from a web page using `requests` and `BeautifulSoup`, then organize it into a structured format using `pandas`. The extracted dataset contains weather records for Chicago in 2017.



**Code Example**

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

**Expected Output**

When the script is executed, a `pandas` DataFrame containing the weather data is displayed. The columns correspond to the table headers, and each row represents a day's weather record.

Example DataFrame Output

```
       Date  High Temp (°F)  Low Temp (°F)  Precipitation (in)
0  01/01/17             38             25                 0.00
1  01/02/17             40             26                 0.02
2  01/03/17             45             28                 0.00
...
```

* Notes: 

- The table is identified by its `id="weather_records"` attribute in the HTML structure.
- `BeautifulSoup` is configured to parse the HTML using the `lxml` parser.
- Data cleaning or transformation (e.g., converting numeric strings to floats) can be performed after the DataFrame creation if necessary.

# 2. SQL Queries

## SQL Query: Number of Trips by Cab Company

This query calculates the number of trips completed by each cab company within a specified date range. The goal is to identify which companies had the highest trip counts over the specified period.

**Use Case**
- **Operational Insights**: Analyzing which cab companies are most active within a specific date range can help in understanding market performance.
- **Business Decision Support**: Can guide promotional strategies, resource allocation, or partnerships with high-performing companies.

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


## SQL Query: Trips Analysis for "Yellow" and "Blue" Cab Companies

This query counts the number of trips completed by cab companies with names containing **"Yellow"** or **"Blue"** over a specified date range. The goal is to identify and compare the performance of these two groups.

**Use Case**
- **Brand Comparison**: Comparing the performance of specific branding (like "Yellow" and "Blue") over a period to inform marketing or operational strategies.
- **Market Segmentation**: Identifying the relative performance of different cab companies based on name or branding, useful for targeting and promotions.

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
    AND cabs.company_name LIKE '%%Yellow%%'
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
    AND cabs.company_name LIKE '%%Blue%%'
GROUP BY company_name;
```

**Key Components**

1. **Filters**:
   - Only includes cab companies whose names contain **"Yellow"** or **"Blue"**.
   - Limits data to trips within the date range of **November 1, 2017**, to **November 7, 2017**.

2. **Grouping**:
   - Groups the trip counts by each company's name.

3. **Union**:
   - Combines results from "Yellow" and "Blue" cab companies into a single output.

### Output Columns

- **company_name**: Name of the cab company.
- **trips_amount**: Total number of trips completed by that company during the specified time.


## SQL Query: Categorizing Taxi Companies into Groups

This SQL query categorizes taxi companies into three groups (`Flash Cab`, `Taxi Affiliation Services`, and `Other`) based on their `company_name` and calculates the number of trips (`trips_amount`) completed by each group within the specified date range. The results are ordered by the total number of trips in descending order.

**Use Case**
- **Competition Analysis**: Helps assess how major players like "Flash Cab" and "Taxi Affiliation Services" compare to other taxi services.
- **Business Strategy**: Identifies market leaders, enabling targeted operational and marketing actions.

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

1. **`CASE` Statement**:
   - Classifies the taxi companies into three groups:
     - `Flash Cab`: Matches exactly to "Flash Cab."
     - `Taxi Affiliation Services`: Matches exactly to "Taxi Affiliation Services."
     - `Other`: All other company names are grouped under "Other."

2. **`COUNT(trips.trip_id)`**:
   - Calculates the total number of trips (`trips.trip_id`) for each company group.

3. **`INNER JOIN`**:
   - Links the `cabs` table to the `trips` table using the `cab_id` column to ensure only relevant records are considered.

4. **`WHERE` Clause**:
   - Filters trips where the start timestamp (`start_ts`) falls between **November 1, 2017**, and **November 7, 2017.**
   - Uses `CAST(trips.start_ts AS date)` to extract the date portion from the timestamp.

5. **`GROUP BY` Clause**:
   - Groups the aggregated trip counts by the newly created `company` column.

6. **`ORDER BY` Clause**:
   - Sorts the results by the number of trips (`trips_amount`) in descending order, showing the most popular group first.

## SQL Query: Extracting Specific Neighborhoods

This SQL query retrieves the `neighborhood_id` and `name` of neighborhoods whose names either contain the substring "Hare" (e.g., "O'Hare") or match exactly "Loop." It uses the `LIKE` operator to perform a pattern match.

**Use Case**
- **Targeted Neighborhood Analysis**: Extracts neighborhoods for targeted reporting or analysis, useful for tourism, urban planning, or business decision-making.
- **Geographical Insights**: Identifying specific neighborhoods, such as "O'Hare" or "Loop," for further exploration in services or development.


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

1. **`SELECT` Clause**:
   - Retrieves:
     - `neighborhood_id`: The unique identifier for each neighborhood.
     - `name`: The name of the neighborhood.

2. **`WHERE` Clause**:
   - Filters the records based on the `name` column using the `LIKE` operator:
     - `name LIKE '%Hare'`: Matches neighborhood names that **end with "Hare"**, such as "O'Hare."
       - `%` is a wildcard that matches zero or more characters before "Hare."
     - `name LIKE 'Loop'`: Matches names that are **exactly "Loop"** (no wildcards used here).

3. **`OR` Logical Operator**:
   - Combines the two conditions, ensuring neighborhoods that satisfy either of the criteria are included in the result.


## SQL Query: Categorizing Weather Conditions

This SQL query categorizes weather conditions as either "Bad" or "Good" based on the presence of specific keywords (`rain` or `storm`) in the weather descriptions. It uses the `CASE` statement for conditional logic.

**Use Case**
- **Weather Impact Analysis**: Simplifies weather data into two categories, aiding in the analysis of its impact on other factors like taxi trip duration or customer behavior.
- **Operational Decision-Making**: Provides an easy way to assess whether weather conditions are favorable for certain activities.

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

1. **`SELECT` Clause**:
   - Retrieves:
     - `ts`: The timestamp column from the `weather_records` table, representing the date and time of the weather record.
     - A calculated column (`weather_conditions`) using the `CASE` statement to classify weather conditions.

2. **`CASE` Statement**:
   - Performs conditional checks on the `description` column:
     - `description LIKE '%rain%'`: Matches descriptions containing the word "rain" (e.g., "light rain," "heavy rain").
     - `description LIKE '%storm%'`: Matches descriptions containing the word "storm" (e.g., "thunderstorm," "ice storm").
   - If either condition is true, the `weather_conditions` column

 is set to 'Bad'; otherwise, it is set to 'Good'.

3. **`LIKE` Operator**:
   - Used for pattern matching to detect keywords within the weather descriptions.












#



This SQL query retrieves the number of trips completed by each cab company between **November 15, 2017**, and **November 16, 2017**. Below is a breakdown of its functionality:


#### Query Breakdown:

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


### Explanation of the SQL Query

This SQL query analyzes the number of trips completed by cab companies whose names include **"Yellow"** or **"Blue"** during the week of **November 1, 2017**, to **November 7, 2017**. It uses the `UNION ALL` operator to combine two separate queries, one for each group of companies.


#### Query Breakdown:

1. **First Query Block**:
   - Filters cab companies with names containing "Yellow" (`cabs.company_name LIKE '%%Yellow%%'`).
   - Counts the number of trips (`trips.trip_id`) for these companies where the trip date falls between **November 1, 2017**, and **November 7, 2017**.
   - Groups results by `company_name`.

2. **Second Query Block**:
   - Filters cab companies with names containing "Blue" (`cabs.company_name LIKE '%%Blue%%'`).
   - Counts the number of trips for these companies within the same date range.
   - Groups results by `company_name`.

3. **UNION ALL**:
   - Combines the results of the two queries into a single output, maintaining duplicate rows if present (e.g., if a cab company somehow meets both criteria).

4. **Output Columns**:
   - `company_name`: Name of the cab company.
   - `trips_amount`: Number of trips completed by that company within the specified date range.



```markdown
## SQL Query: Trips Analysis for "Yellow" and "Blue" Cab Companies

This query counts the number of trips completed by cab companies with names containing **"Yellow"** or **"Blue"** over a specified date range. The goal is to identify and compare the performance of these two groups.

### Query

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
    AND cabs.company_name LIKE '%%Yellow%%'
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
    AND cabs.company_name LIKE '%%Blue%%'
GROUP BY company_name;
```

### Key Components

1. **Filters**:
   - Only includes cab companies whose names contain **"Yellow"** or **"Blue"**.
   - Limits data to trips within the date range of **November 1, 2017**, to **November 7, 2017**.

2. **Grouping**:
   - Groups the trip counts by each company's name.

3. **Union**:
   - Combines results from "Yellow" and "Blue" cab companies into a single output.

### Output Columns

- **company_name**: Name of the cab company.
- **trips_amount**: Total number of trips completed by that company during the specified time.

### Use Case

This query can be used to compare the performance of cab companies with specific branding ("Yellow" or "Blue") over a given period. It is useful for operational analysis, marketing insights, and identifying top-performing companies within these two groups.
```

### SQL Query Explanation and Markdown Presentation

This SQL query categorizes taxi companies into three groups (`Flash Cab`, `Taxi Affiliation Services`, and `Other`) based on their `company_name` and calculates the number of trips (`trips_amount`) completed by each group within the specified date range. The results are ordered by the total number of trips in descending order.

### Query

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

### Key Components

1. **`CASE` Statement**:
   - Classifies the taxi companies into three groups:
     - `Flash Cab`: Matches exactly to "Flash Cab."
     - `Taxi Affiliation Services`: Matches exactly to "Taxi Affiliation Services."
     - `Other`: All other company names are grouped under "Other."

2. **`COUNT(trips.trip_id)`**:
   - Calculates the total number of trips (`trips.trip_id`) for each company group.

3. **`INNER JOIN`**:
   - Links the `cabs` table to the `trips` table using the `cab_id` column to ensure only relevant records are considered.

4. **`WHERE` Clause**:
   - Filters trips where the start timestamp (`start_ts`) falls between **November 1, 2017**, and **November 7, 2017.**
   - Uses `CAST(trips.start_ts AS date)` to extract the date portion from the timestamp.

5. **`GROUP BY` Clause**:
   - Groups the aggregated trip counts by the newly created `company` column.

6. **`ORDER BY` Clause**:
   - Sorts the results by the number of trips (`trips_amount`) in descending order, showing the most popular group first.

### Use Case

This query is useful for:
- **Analyzing Competition**: Understanding the trip volumes of top-performing taxi companies (`Flash Cab` and `Taxi Affiliation Services`) compared to the rest.
- **Operational Insights**: Identifying major contributors to the overall trip count within a given timeframe.
- **Decision-Making**: Focusing marketing or operational improvements on high-performing companies or those in the "Other" category to boost performance.

### SQL Query Explanation and Markdown Presentation

This SQL query retrieves the `neighborhood_id` and `name` of neighborhoods whose names either contain the substring "Hare" (e.g., "O'Hare") or match exactly "Loop." It uses the `LIKE` operator to perform a pattern match.

### Query

```sql
SELECT
    neighborhood_id,
    name
FROM 
    neighborhoods
WHERE 
    name LIKE '%Hare' OR name LIKE 'Loop';
```

### Key Components

1. **`SELECT` Clause**:
   - Retrieves the following columns from the `neighborhoods` table:
     - `neighborhood_id`: The unique identifier for each neighborhood.
     - `name`: The name of the neighborhood.

2. **`WHERE` Clause**:
   - Filters the records based on the `name` column using the `LIKE` operator:
     - `name LIKE '%Hare'`: Matches neighborhood names that **end with "Hare"**, such as "O'Hare."
       - `%` is a wildcard that matches zero or more characters before "Hare."
     - `name LIKE 'Loop'`: Matches names that are **exactly "Loop"** (no wildcards used here).

3. **`OR` Logical Operator**:
   - Combines the two conditions, ensuring neighborhoods that satisfy either of the criteria are included in the result.


### Use Case

This query is useful for:
- **Targeted Neighborhood Analysis**: Extracting specific neighborhoods for further analysis, such as "O'Hare" and "Loop," which are prominent in Chicago.
- **Filtering for Custom Reporting**: Generating reports focused on neighborhoods matching specific patterns or names.
- **Geographical Insights**: Identifying key areas of interest for urban planning, tourism, or service coverage.

### Notes

- The query results depend on the specific data in the `neighborhoods` table.
- Use `%` or `_` in the `LIKE` operator to broaden or refine the pattern-matching criteria:
  - `%`: Matches any sequence of characters.
  - `_`: Matches a single character.

  ### SQL Query Explanation and Markdown Presentation

This SQL query categorizes weather conditions as either "Bad" or "Good" based on the presence of specific keywords (`rain` or `storm`) in the weather descriptions. It uses the `CASE` statement for conditional logic.

### Query

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

### Key Components

1. **`SELECT` Clause**:
   - Retrieves:
     - `ts`: The timestamp column from the `weather_records` table, representing the date and time of the weather record.
     - A calculated column (`weather_conditions`) using the `CASE` statement to classify weather conditions.

2. **`CASE` Statement**:
   - Performs conditional checks on the `description` column:
     - `description LIKE '%rain%'`: Matches descriptions containing the word "rain" (e.g., "light rain," "heavy rain").
     - `description LIKE '%storm%'`: Matches descriptions containing the word "storm" (e.g., "thunderstorm," "ice storm").
   - If either condition is true, the `weather_conditions` column is assigned the value `'Bad'`.
   - If neither condition is true, the value `'Good'` is assigned.

3. **`FROM` Clause**:
   - Specifies the table (`weather_records`) from which the data is retrieved.

### Use Case

This query is useful for:
- **Weather Impact Analysis**: Categorizing weather into binary states to assess its effect on other variables, such as ride durations or traffic.
- **Simplified Reporting**: Creating a summary of weather conditions for easy visualization or further aggregation.
- **Operational Decision-Making**: Informing whether conditions are favorable for certain activities based on historical weather patterns.

### Notes

- The `%` wildcard in the `LIKE` operator allows partial matches, ensuring flexibility in identifying keywords.
- Additional weather conditions can be classified by expanding the `CASE` statement with more `WHEN` clauses. For example, snowy conditions could be added as another category.

### SQL Query Explanation and Markdown Presentation

This query analyzes taxi trips under specific conditions and joins weather data to assess its relationship with trip characteristics.

### Query

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

### Key Components

1. **Subquery (`T`)**:
   - Extracts `ts` (timestamp) and categorizes weather conditions as "Bad" (rain or storm in `description`) or "Good" (otherwise) using a `CASE` statement.
   - Represents weather data for each timestamp.

2. **`INNER JOIN`**:
   - Joins the `trips` table with the subquery `T` on matching timestamps (`T.ts = trips.start_ts`).
   - Ensures that only trips occurring at the same time as recorded weather conditions are included.

3. **`SELECT` Clause**:
   - Retrieves:
     - `start_ts`: The timestamp when the trip began.
     - `T.weather_conditions`: The weather conditions ("Bad" or "Good") associated with the trip's start time.
     - `duration_seconds`: The duration of the trip in seconds.

4. **`WHERE` Clause**:
   - Filters trips:
     - `pickup_location_id = 50`: The trip starts from location ID 50.
     - `dropoff_location_id = 63`: The trip ends at location ID 63.
     - `EXTRACT(DOW FROM trips.start_ts) = 6`: The trip starts on a Saturday (`DOW` = Day of the Week, where 0 = Sunday, 6 = Saturday).

5. **`ORDER BY` Clause**:
   - Orders the results by `trip_id` for better organization.

### Use Case

This query is useful for:
1. **Weather Impact Analysis**:
   - Understanding how weather conditions affect trip duration for specific routes and times.
2. **Operational Planning**:
   - Identifying high-risk conditions (e.g., "Bad" weather) to allocate resources or adjust pricing strategies.
3. **Pattern Identification**:
   - Analyzing route performance during particular weather conditions on weekends.

### Notes

- The subquery ensures the weather conditions are pre-classified and ready to join with the trip data.
- `EXTRACT(DOW FROM trips.start_ts)` focuses the analysis on Saturdays, which may be of interest for weekend traffic or demand studies.
- This query can be expanded by including other trip characteristics, such as fare or passenger count, to deepen the analysis.