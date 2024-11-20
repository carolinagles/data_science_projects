

# Store One - Data Cleaning and Analysis in E-commerce

This project focuses on analyzing and cleaning data collected by the e-commerce store **Store One**. Key Python concepts, including data types, loops, lists, and string formatting, are applied to transform and prepare the dataset for future analysis.

## Objective
The goal is to evaluate the quality of the dataset, correct inconsistencies, and perform necessary transformations using Python programming techniques.

---

## Concepts Applied

### Data Types and Conversion
- The raw data includes types like `int`, `float`, `string`, and `list`.  
- Incorrect data types were identified and corrected:
  - `user_id` was converted from `string` to `integer`.
  - `user_age` was corrected from `float` to `integer`.

### String Manipulation
- Methods used include:
  - `strip()` to remove unnecessary spaces.
  - `replace()` to fix characters in names.
  - `lower()` to standardize strings in lists (`fav_categories`).
- Advanced string formatting with f-strings for clarity and customization.

### Lists
- Transformation of lists like `fav_categories` to ensure consistency in values.  
- Application of slicing and index-based access operations.

### Loops and Conditional Statements
- Use of `for` loops to iterate over lists and apply transformations.
- Conditional statements implemented to detect and correct data errors.

### Error Handling
- `try-except` blocks used to handle invalid data and prevent execution interruptions.

---


## Key Python Resources
This project demonstrates the use of the following Python resources:
- Data conversion and cleaning with `int()`, `float()`, `str()`.
- String manipulation with `strip()`, `replace()`, `lower()`, and f-strings.
- Iteration with `for` and validation with `if-else`.
- Error prevention with `try-except`.

---

**Note:** This project is ideal for beginners looking to apply basic programming concepts to data cleaning and preparation.

