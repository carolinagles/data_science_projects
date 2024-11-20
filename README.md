# Instacart Orders  -  Data Wrangling

## Project Description

For this project, data from Instacart will be worked with.

Instacart is a grocery delivery platform where customers can place an order and have it delivered, similar to Uber Eats and Door Dash. This particular dataset was publicly released by Instacart in 2017 for a Kaggle competition. The actual data can be downloaded directly from the competition page on Kaggle.

The dataset provided has modifications from the original. The dataset size was reduced to make calculations faster, and missing and duplicate values were introduced. Care was taken to maintain the original data distributions when these changes were made.

The goal is to clean the data and prepare a report that provides insights into the shopping habits of Instacart customers.

This project will require creating graphs to communicate the results. Ensure that each graph created has a title, labeled axes, and a legend if necessary, and include `plt.show()` at the end of each cell containing a graph.

## Data Dictionary

There are five tables in the dataset, and all will be used for data preprocessing and exploratory data analysis. Below is a data dictionary listing the columns in each table and describing the data they contain.

### instacart_orders.csv
Each row corresponds to an order in the Instacart application.

- `order_id`: A unique identifier for each order.
- `user_id`: A unique identifier for each customer account.
- `order_number`: The number of orders this customer has placed.
- `order_dow`: The day of the week the order was placed (0 if it's Sunday).
- `order_hour_of_day`: The hour of the day the order was placed.
- `days_since_prior_order`: The number of days since the customer's previous order.

### products.csv
Each row corresponds to a unique product that can be purchased by customers.

- `product_id`: A unique identifier for each product.
- `product_name`: The name of the product.
- `aisle_id`: A unique identifier for each grocery aisle category.
- `department_id`: A unique identifier for each grocery department.

### order_products.csv
Each row corresponds to an item ordered in an order.

- `order_id`: A unique identifier for each order.
- `product_id`: A unique identifier for each product.
- `add_to_cart_order`: The sequential order in which each item was added to the cart.
- `reordered`: 0 if the customer has never ordered this product before, 1 if it has been ordered before.

### aisles.csv
Each row corresponds to a grocery aisle category.

- `aisle_id`: A unique identifier for each aisle category.
- `aisle`: The name of the aisle.

### departments.csv
Each row corresponds to a grocery department.

- `department_id`: A unique identifier for each department.
- `department`: The name of the department.

## Instructions to Complete the Project

### Step 1: Open the data files and take a look at the general content of each table.

Note that the files have a non-standard format, so certain arguments need to be set in `pd.read_csv()` to read the data correctly. Review the CSV files to get an idea of what arguments should be used.


#### Step 2: Preprocess the data as follows:

- Verify and correct data types (e.g., ensure that ID columns are integers).
- Identify and fill missing values.
- Identify and remove duplicate values.
- Explain what types of missing values and duplicates were found, how they were filled or removed, and why these methods were used. Why do you think these missing and duplicate values might have been present in the dataset?

#### Step 3: Once the data is processed and ready, perform the following analysis:

**[A]** 

- Verify that the values in the columns `order_hour_of_day` and `order_dow` in the `orders` table are reasonable (i.e., `order_hour_of_day` should be between 0 and 23, and `order_dow` should be between 0 and 6).
- Create a graph showing the number of people who place orders depending on the hour of the day.
- Create a graph showing which day of the week people make their purchases.
- Create a graph showing how long people wait before placing their next order and comment on the minimum and maximum values.

**[B]** 

- Is there a difference in the distributions of `order_hour_of_day` on Wednesdays and Saturdays? Plot the histograms for both days on the same graph and describe any differences observed.
- Plot the distribution of the number of orders placed by customers (e.g., how many customers placed only one order, how many placed two, how many placed three, etc.).
- What are the 20 most frequently ordered products (show their IDs and names)?

**[C]** 

- How many items does a person generally buy in a single order? What is the distribution like?
- What are the 20 most frequently reordered items (show their names and product IDs)?
- For each product, what proportion of its orders are reordered (create a table with columns for product ID, product name, and reorder proportion)?
- What is the reorder proportion for each customer?
- What are the 20 items that people add to their carts first (show their product IDs, names, and how many times they were the first item added to the cart)?

---