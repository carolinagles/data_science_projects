# BASIC PYTHON (E-COMMERCE)

In this project, we’re working with various data types and their functionalities, building a robust understanding of Python’s core features to optimize our workflow.

1. **Data Types**:  
   We deal with foundational types such as `int` (integer), `float` (decimal numbers), and `bool` (True/False). Complex types like `str` (strings) and `list` (arrays) allow us to handle text and collections effectively. For example:
   - Integer values might represent a year (`Year_of_Release`).
   - Strings help store game names or genres (`Name`, `Genre`).
   - Lists are handy for grouping multiple games under analysis.

2. **Type Conversion and Operations**:  
   In our analysis, we often need to convert data types. For instance:
   - Converting `Year_of_Release` from string to integer for sorting.
   - Using arithmetic operations on sales columns (e.g., `NA_sales + EU_sales`) to calculate total sales.

3. **Error Handling**:  
   To prevent interruptions during data analysis, we use the `try-except` block. This ensures smooth execution when dealing with missing or corrupt data. For instance:
   ```python
   try:
       average_score = float(critic_score)
   except ValueError:
       average_score = None
   ```

4. **String Manipulation**:  
   Managing textual data is essential. Methods like:
   - `.upper()` or `.lower()` help standardize strings.
   - `.strip()` removes unwanted spaces, ensuring cleaner data.
   - `.replace()` is useful for correcting typos or formatting strings.

   Example:  
   ```python
   genre = genre.replace('Action ', 'Action').strip()
   ```

5. **Lists and Indexing**:  
   Lists store collections like selected platforms (`['PS3', 'X360', 'PS4']`). Using indexing and slicing, we extract specific segments. For instance:
   ```python
   platforms[:2]  # Accesses 'PS3' and 'X360'
   ```

6. **Loops**:  
   Loops automate repetitive tasks. For instance:
   - A `for` loop iterates over a list of games to process each entry.
   - A `while` loop can validate user input until a correct value is provided.

   Example:  
   ```python
   for platform in platforms:
       print(f"Analyzing {platform} data...")
   ```

7. **Conditional Statements**:  
   Branching logic customizes actions based on data conditions. For example:
   ```python
   if total_sales > 1_000_000:
       category = "Hit"
   else:
       category = "Average"
   ```

By combining these tools, we create a powerful, flexible codebase for efficient data analysis and ensure adaptability for handling diverse datasets and challenges.