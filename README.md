# Car Sales Data Cleaning Notebook

This notebook performs data cleaning on a car sales dataset to prepare it for further analysis or visualization. The process involves loading, cleaning, and processing the dataset to ensure it's ready for use in data visualizations or predictive modeling.

## Steps Involved

### 1. **Import Dependencies**

We begin by importing the required Python libraries:
- `pandas`: For data manipulation and analysis.

```python
import pandas as pd

2. Load and Read CSV File
The car sales dataset is loaded from a CSV file located on the local machine. We use the pd.read_csv() function to read the file into a DataFrame.

data = ('/Users/chac/Desktop/Data/car_price/Data/car_prices.csv')
df = pd.read_csv(data)

3. Display Dataset
The DataFrame df is displayed to get an initial look at the data.
df

4. Remove Timezone Information (if not needed)
If the saledate column contains timezone information, we remove it using a regular expression. This step ensures the dates are in a uniform format without additional timezone details.
df['saledate'] = df['saledate'].str.replace(r' GMT.*', '', regex=True)

5. Convert saledate to String
To avoid any mixed data types that may arise, we convert the saledate column to a string type.
df['saledate'] = df['saledate'].astype(str)

6. Convert saledate to Datetime
We attempt to convert the saledate column into a datetime object. Any rows where the conversion fails will be marked as NaT (Not a Time).
df['saledate'] = pd.to_datetime(df['saledate'], errors='coerce')

7. Identify Invalid Date Entries
After attempting the conversion, any invalid date entries are identified by checking for NaT values in the saledate column.
invalid_dates = df[df['saledate'].isna()]
The invalid entries are printed for further inspection.
print("\nInvalid date entries:")
print(invalid_dates)

8. Remove Rows with Invalid Dates
To ensure the data is clean, rows with invalid date entries are dropped. The cleaned data is stored in a new DataFrame called df_cleaned.
df_cleaned = df.dropna(subset=['saledate']).copy()  # Create a cleaned copy

9. Extract Year from saledate
The year of the sale is extracted from the saledate column and added as a new column called purchase_year.
df_cleaned['purchase_year'] = df_cleaned['saledate'].dt.year

10. Display Cleaned Data
The cleaned DataFrame with the newly added purchase_year column is displayed.
print("\nCleaned DataFrame:")
print(df_cleaned[['saledate', 'purchase_year']])

11. Remove Null/NaN Values
Any remaining rows with missing values are removed from the df_cleaned DataFrame.
df_cleaned = df_cleaned.dropna()

12. Export Cleaned Data
Finally, the cleaned dataset is saved as a new CSV file for future use, such as visualization or further analysis.
df_cleaned.to_csv('/Users/chac/Desktop/Data/car_price/Outputs/car_sales_cleaned.csv', index=False)

Output
After running the notebook, the cleaned dataset is saved as car_sales_cleaned.csv in the specified output directory.
Requirements
This notebook requires Python 3 and the following Python packages:
* pandas
You can install the necessary package using pip:
pip install pandas