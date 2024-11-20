# Video Game Sales Analysis Project

## Project Objective

The objective of this project is to identify patterns that determine whether a video game is successful or not. This will enable the detection of promising projects and the planning of advertising campaigns. The analysis is based on historical data available since 2016, assuming it is December 2016, and a campaign for 2017 is being planned.

The dataset includes information such as user and expert reviews, game genres, platforms (e.g., Xbox or PlayStation), and sales data. The goal is to gain experience working with data, and the focus is not necessarily on forecasting sales for 2017 based on 2016 data, or even sales for 2027 based on 2026 data.

The dataset contains a column called "rating" which stores the ESRB classification for each game. The Entertainment Software Rating Board (ESRB) evaluates the content of a game and assigns an age rating, such as Teen or Adult.

## Dataset Description

- **Name**: Name of the game
- **Platform**: Platform (e.g., Xbox, PlayStation)
- **Year_of_Release**: Year of release
- **Genre**: Genre of the game
- **NA_sales**: Sales in North America (in millions of USD)
- **EU_sales**: Sales in Europe (in millions of USD)
- **JP_sales**: Sales in Japan (in millions of USD)
- **Other_sales**: Sales in other countries (in millions of USD)
- **Critic_Score**: Maximum score of 100
- **User_Score**: Maximum score of 10
- **Rating**: ESRB rating (e.g., Teen, Adult)

## Libraries Used

These libraries were imported to facilitate data manipulation, statistical analysis, and data visualization throughout the project. The libraries include `pandas` for data handling, `numpy` for numerical operations, `matplotlib` and `seaborn` for visualizations, `plotly` for interactive plots, and `scipy` for statistical tests like the t-test.


## Instructions for Completing the Project

### Step 1: Open the Data File and Study the General Information

File path:

- `games.csv` Download the dataset.

### Step 2: Prepare the Data

- Replace the column names (convert them to lowercase).
- Convert the data to the necessary types.
- Describe the columns where the data types were changed and explain why.
- If necessary, decide how to handle missing values:
  - Explain why missing values were handled in a particular way or why they were left blank.
  - Consider possible reasons why values are missing.
  - Pay attention to the abbreviation TBD: "to be determined." Specify how these cases should be handled.
- Calculate the total sales (sum of sales across all regions) for each game and place these values in a separate column.

### Step 3: Analyze the Data

- Check how many games were released in different years. Are the data for each period significant?
- Observe how sales vary across platforms. Choose platforms with the highest total sales and create a distribution based on data for each year. Look for platforms that were once popular but now have no sales. How long does it typically take for new platforms to appear and old ones to disappear?
- Determine which time period the data should cover. This will depend on the answers to the previous questions. The data should allow for the construction of a model for 2017.
- Focus only on the data you consider relevant. Ignore data from previous years.
- Which platforms are leading in sales? Which ones are growing and which are declining? Select several potentially profitable platforms.
- Create a box plot for global sales of all games, broken down by platform. Are there significant differences in sales? What happens to average sales on different platforms? Describe your findings.
- Look at how both user and critic reviews affect sales on a popular platform (your choice). Create a scatter plot and calculate the correlation between reviews and sales. Draw conclusions.
- Considering your conclusions, compare the sales of the same games on different platforms.
- Look at the general distribution of games by genre. What can be said about the most profitable genres? Can generalizations be made about genres with high and low sales?

### Step 4: Create a User Profile for Each Region

For each region (NA, EU, JP), determine:

- The top five platforms. Describe the variations in their market share from one region to another.
- The top five genres. Explain the differences.
- Whether ESRB ratings affect sales in individual regions.

### Step 5: Test the Following Hypotheses

- The average user ratings for Xbox One and PC platforms are the same.
- The average user ratings for the Action and Sports genres are different.

Set your own threshold for alpha.

Explain:

- How the null and alternative hypotheses were formulated.
- The criterion used to test the hypotheses and why.

### Step 6: Write a General Conclusion


--- 