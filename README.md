# FIFA-Data-Cleaning-Project


![20250604_2341_Data Cleaning Visualization_simple_compose_01jwyapsyqemwa8pmbgasvvcs3](https://github.com/user-attachments/assets/09ea4935-3c16-4127-893f-287a5e36eb82)


## Introduction

They say **data is the new oil**, but like crude oil, **raw data is rarely usable in its original form**. It’s often messy, inconsistent, and incomplete, and using it without proper cleaning can lead to misleading insights or flawed decisions. This is why **data cleaning is one of the most critical steps** in any data project. It ensures that what we analyze is accurate, consistent, and trustworthy.

In this project, I worked on cleaning a FIFA player dataset that, while rich in information, came with its fair share of issues from special characters in names to inconsistent units of measurement, star symbols, missing values, and irregular date formats.

The dataset contains **18,979 rows and 77 columns**, covering a wide range of player attributes such as names, physical stats, market values, ratings, nationalities, and more.

The goal of this project was to **clean and prepare the dataset for reliable analysis**, transforming it into a format that supports accurate interpretation and modeling.

*  **Raw and cleaned versions** of the dataset can be found in the [`data`](./data) folder.
*  The cleaning process was performed using **Power Query in Microsoft Excel**.
*  The dataset was originally provided in **CSV format**, which preserved the data structure but introduced encoding and formatting challenges.

---

## Tools Used

* **Power Query (in Excel)** – for data cleaning and transformation.
* **Microsoft Excel** – for additional validation and inspection.
* **CSV file** – original data source.

---

## Tasks and Steps Performed

### 1. **Correct Special Characters in Text Fields**

#### Issue:

Some player and club names contained corrupted characters (e.g., "Ã©" instead of "é") due to encoding issues.

<img width="960" alt="Names_Inconsistencies d" src="https://github.com/user-attachments/assets/6a34c63c-4482-4365-8122-f71adc7e848b" />


#### Solution:

* **Step 1**: Open the CSV file with proper UTF-8 encoding.
* **Step 2**: Use Power Query’s **‘Replace Values’** to correct remaining special characters manually where necessary.
* **Outcome**: Names were displayed correctly (e.g., "Kylian Mbappé" instead of "Kylian MbappÃ©").


<img width="960" alt="Trim text c" src="https://github.com/user-attachments/assets/534ddc58-b8f3-4c61-b249-5389d0be3572" />


**Validation**: Importing as CSV with UTF-8 encoding automatically resolved most encoding issues. Manual intervention was minimal and only required for a few residual cases.

---

### 2. **Standardize Height and Weight Measurements** 

#### Issue:

Inconsistent units:

* Height in **feet/inches** (e.g., "5'9") and **cm**
* Weight in **lbs** and **kg**


<img width="960" alt="height weight d" src="https://github.com/user-attachments/assets/19cdb68f-d388-4e94-9134-c8c6c35a5dbf" />


#### Solution:

* **Step 1**: Identify units using conditional logic.
* **Step 2**: For height in feet/inches, split the string and convert to centimeters using:
  Height_cm = (Feet \* 30.48) + (Inches \* 2.54)
* **Step 3**: For weight in lbs, convert using:
  Weight_kg = Weight_lbs * 0.453592
* **Step 4**: Replace original columns with new standardized versions.


<img width="960" alt="height weight c" src="https://github.com/user-attachments/assets/6ce5bea0-ce81-4147-8865-ae91702d60d4" />

 
 **Outcome**: All height values were converted to centimeters, and weight values to kilograms for consistency and easier comparison.

---

### 3. **Convert Monetary Values to Numeric** 

#### Issue:

Columns like `Value`, `Wage`, and `Release Clause` had values like "€5M", "€200K".


<img width="960" alt="wage d" src="https://github.com/user-attachments/assets/2051518b-9c00-41c8-ba34-898e20c316a8" />


#### Solution:

* **Step 1**: Strip currency symbols and suffixes (e.g., €, M, K).
* **Step 2**: Replace "M" with \*1,000,000 and "K" with \*1,000.
* **Step 3**: Convert final cleaned values to numeric.


<img width="960" alt="wage c" src="https://github.com/user-attachments/assets/10eed5f9-a2c2-4964-936a-9c4183017c6a" />


**Outcome**: All financial figures are now in numeric format for accurate aggregation and comparison.

---

### 4. **Handle Star Ratings**

#### Issue:

Column `IR` (International Reputation) used star ratings (e.g., "4★").


<img width="960" alt="star ratings d" src="https://github.com/user-attachments/assets/e9fedd2e-f785-4c2e-bf5c-52d6a5a85570" />


#### Solution:

* **Step 1**: Split column at the star symbol ("★").
* **Step 2**: Retain only the numeric portion.
* **Step 3**: Rename abbreviations to full column names.
* **Step 4**: Change data type to numeric.



<img width="960" alt="star ratings c" src="https://github.com/user-attachments/assets/39336159-eae4-42aa-b154-b66d15c44ef3" />


**Outcome**: Star rating columns are now clean numeric fields ready for analysis.

---

### 5. **Standardize Date Formats**

#### Issue:

Date columns like `Contract Valid Until` had mixed formats:

* Some were complete dates.
* Others had free text like “On loan”, “Free”, “2015 ~ 2021”.

#### Solution:

* `Joined`: Already in valid date format – no changes made.
* `Contract Valid Until`: Due to mixed formats, this column was **left unchanged** to prevent incorrect parsing.

**Validation**: Inconsistent formats like "on loan" or ranges ("2015\~2021") are categorical in nature and not suited for conversion to a single date format.

---

### 6. **Address Missing and Null Values**

#### Columns with Missing Values:

1. `Loan End Date`
2. `Hits`

#### Solution:

1. **Loan End Date**:

  * Missing values indicated players **not currently on loan**.
  * **Filled with**: `"Not on loan"`

   **Validation**: This categorically defines player status and maintains analytical meaning.

2. **Hits**:

  * Missing likely means **no recorded popularity/hit data**.
  * **Filled with**: `"No hits"`

  **Validation**: Retains the record without assuming numeric zero; reflects lack of data rather than performance.

---

### 7. **Remove Unnecessary Whitespace**

#### Issue:

Extra spaces at the beginning or end of strings in various text fields.


<img width="960" alt="trim text d" src="https://github.com/user-attachments/assets/c535c814-a69a-4d72-8efe-d23965b59f10" />


#### Solution:

* Used Power Query’s **Text.Trim()** to clean all text fields.


<img width="960" alt="Trim text c" src="https://github.com/user-attachments/assets/9d0671f7-3378-47ac-b286-4cf26b93cd57" />


**Outcome**: Clean categorical/text data without whitespace inconsistencies.

---

### 8. **Ensure Consistent Categorical Data**

#### Fields Checked:

* `Position`
* `Nationality`

#### Outcome:

* After inspection, **no typos or variations** were found. No action was necessary.

---

### 9. **Rename Columns for Clarity**

#### Example:

* `↓OVA` was renamed to `Overall Rating`.

**Outcome**: Improves column readability and interpretation for users unfamiliar with abbreviations.

---

## Additional Checks Performed

* Verified that all numeric columns were of appropriate data types after cleaning.
* Ensured categorical and date columns were not incorrectly parsed during transformation.

---

## Conclusion

This project involved a thorough cleaning of FIFA player data using Power Query. From handling inconsistent units and special characters to standardizing monetary formats and addressing missing values, each step was guided by careful validation and clear logic. The final cleaned dataset is ready for exploratory data analysis or predictive modeling.

