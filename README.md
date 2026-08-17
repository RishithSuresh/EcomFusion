# 🛒 EcomFusion — E-commerce Data Integration & Processing

## 📌 Project Overview

**EcomFusion** is a Python-based data processing project that combines three related e-commerce datasets — **Orders, Customers, and Products** — into a single clean and meaningful dataset using **Pandas**.

The project demonstrates essential data-processing techniques such as loading CSV files, inspecting datasets, merging related DataFrames, concatenating DataFrames, transforming data using `apply()`, performing DateTime operations, creating derived features, organizing the final dataset, and exporting the processed data as a CSV file.

The resulting dataset can serve as a foundation for further **Exploratory Data Analysis (EDA), data visualization, business intelligence, and machine learning** tasks.

---

## 🎯 Objectives

The main objectives of this project are:

* Load multiple CSV datasets using Pandas.
* Explore the structure and contents of each dataset.
* Combine related datasets using `merge()`.
* Demonstrate DataFrame combination using `concat()`.
* Use `apply()` to create useful derived columns.
* Convert order dates into Pandas DateTime format.
* Extract year, month, day, and day of the week from order dates.
* Calculate the total amount of each order.
* Organize the combined information into a clean DataFrame.
* Check for missing and duplicate values.
* Export the final processed dataset as a CSV file.

---

## 🗂️ Datasets

The project uses three CSV files:

### 1. Orders

**File:** `Day9_Orders.csv`

Contains information about customer orders, including:

* Order ID
* Order Date
* Customer ID
* Product ID
* Quantity
* Payment Method
* Order Status

### 2. Customers

**File:** `Day9_Customers.csv`

Contains customer-related information, including:

* Customer ID
* Customer Name
* City
* Region
* Membership Type

### 3. Products

**File:** `Day9_Products.csv`

Contains product-related information, including:

* Product ID
* Product Name
* Category
* Unit Price
* Brand

---

## 🔗 Dataset Relationships

The datasets are connected through common identifiers.

```text
                 ┌─────────────────┐
                 │    Customers    │
                 │                 │
                 │  Customer_ID    │
                 └────────┬────────┘
                          │
                          │ Customer_ID
                          ▼
┌─────────────────┐     ┌─────────────────┐
│    Products     │     │      Orders     │
│                 │     │                 │
│   Product_ID    │◄────│   Product_ID    │
│   Product_Name  │     │   Customer_ID   │
│   Category      │     │   Quantity      │
│   Unit_Price    │     │   Order_Date    │
└─────────────────┘     └─────────────────┘
```

The `Orders` dataset acts as the central dataset and is connected to:

* `Customers` using `Customer_ID`
* `Products` using `Product_ID`

---

## 🛠️ Technologies Used

* **Python**
* **Pandas**
* **NumPy**
* **Google Colab**
* **Jupyter Notebook**

---

## 📂 Project Structure

```text
EcomFusion/
│
├── Day9_Orders.csv
├── Day9_Customers.csv
├── Day9_Products.csv
├── EcomFusion.ipynb
├── Processed_Ecommerce_Dataset.csv
└── README.md
```

---

# 🔄 Data Processing Workflow

The overall workflow of the project is:

```text
Orders.csv
    │
    ├──────────────┐
    │              │
    ▼              ▼
Customers.csv   Products.csv
    │              │
    └──────┬───────┘
           │
           ▼
        merge()
           │
           ▼
    Combined Dataset
           │
           ├── DateTime Processing
           │
           ├── apply()
           │
           ├── Derived Features
           │
           ├── Data Cleaning
           │
           ▼
    Final Processed DataFrame
           │
           ▼
Processed_Ecommerce_Dataset.csv
```

---

# 🔍 Processing Steps

## 1. Load the Datasets

The three CSV files are loaded using Pandas:

```python
orders = pd.read_csv("Day9_Orders.csv")
customers = pd.read_csv("Day9_Customers.csv")
products = pd.read_csv("Day9_Products.csv")
```

---

## 2. Inspect the Datasets

The datasets are examined using functions such as:

```python
orders.head()
customers.head()
products.head()
```

Their dimensions and column names are also inspected using:

```python
orders.shape
customers.shape
products.shape
```

and:

```python
orders.columns
customers.columns
products.columns
```

---

## 3. DateTime Processing

The `Order_Date` column is converted into Pandas DateTime format:

```python
orders["Order_Date"] = pd.to_datetime(
    orders["Order_Date"],
    errors="coerce"
)
```

Additional date-related features are then created:

```python
orders["Order_Year"] = orders["Order_Date"].dt.year

orders["Order_Month"] = orders["Order_Date"].dt.month

orders["Month_Name"] = orders["Order_Date"].dt.month_name()

orders["Order_Day"] = orders["Order_Date"].dt.day

orders["Day_of_Week"] = orders["Order_Date"].dt.day_name()
```

This allows the order data to be analyzed based on different time periods.

---

## 4. Using `apply()`

The project uses `apply()` to categorize orders based on quantity.

```python
def quantity_category(quantity):
    if quantity <= 2:
        return "Low"
    elif quantity <= 4:
        return "Medium"
    else:
        return "High"

orders["Quantity_Category"] = orders["Quantity"].apply(
    quantity_category
)
```

This creates a new feature called:

```text
Quantity_Category
```

with categories such as:

* Low
* Medium
* High

---

## 5. Merging Customers with Orders

The customer information is added to the Orders dataset using `merge()`:

```python
processed_df = pd.merge(
    orders,
    customers,
    on="Customer_ID",
    how="left"
)
```

---

## 6. Merging Products

Product information is then added using `Product_ID`:

```python
processed_df = pd.merge(
    processed_df,
    products,
    on="Product_ID",
    how="left"
)
```

The resulting DataFrame now contains information from all three datasets.

---

## 7. Calculating Total Amount

The project's processed dataset calculates the total order value using:

```text
Total Amount = Quantity × Unit Price
```

This is implemented using `apply()`:

```python
processed_df["Total_Amount"] = processed_df.apply(
    lambda row: row["Quantity"] * row["Unit_Price"],
    axis=1
)
```

---

## 8. Using `concat()`

The project also demonstrates combining DataFrames using `concat()`.

The Orders DataFrame is divided into two portions:

```python
orders_part1 = orders.iloc[:60]
orders_part2 = orders.iloc[60:]
```

They are then combined:

```python
orders_combined = pd.concat(
    [orders_part1, orders_part2],
    ignore_index=True
)
```

This demonstrates vertical concatenation of DataFrames.

---

# 📊 Final Dataset

The final processed dataset organizes information from all three source datasets into a single DataFrame.

The final features include:

| Feature             | Description                       |
| ------------------- | --------------------------------- |
| `Order_ID`          | Unique order identifier           |
| `Order_Date`        | Date of the order                 |
| `Order_Year`        | Year of the order                 |
| `Order_Month`       | Numerical month                   |
| `Month_Name`        | Name of the month                 |
| `Order_Day`         | Day of the month                  |
| `Day_of_Week`       | Day on which the order occurred   |
| `Customer_ID`       | Customer identifier               |
| `Customer_Name`     | Customer name                     |
| `City`              | Customer's city                   |
| `Region`            | Customer region                   |
| `Membership_Type`   | Customer membership category      |
| `Product_ID`        | Product identifier                |
| `Product_Name`      | Product name                      |
| `Category`          | Product category                  |
| `Brand`             | Product brand                     |
| `Unit_Price`        | Price per unit                    |
| `Quantity`          | Quantity purchased                |
| `Quantity_Category` | Low/Medium/High quantity category |
| `Total_Amount`      | Total value of the order          |
| `Payment_Method`    | Payment method used               |
| `Order_Status`      | Current order status              |

---

# 🧹 Data Quality Checks

The final dataset is checked for:

### Missing Values

```python
final_df.isnull().sum()
```

### Duplicate Records

```python
final_df.duplicated().sum()
```

### Data Types

```python
final_df.dtypes
```

### Dataset Dimensions

```python
final_df.shape
```

These checks help ensure that the processed dataset is suitable for further analysis.

---

# 📤 Exporting the Dataset

The final DataFrame is exported using:

```python
final_df.to_csv(
    "Processed_Ecommerce_Dataset.csv",
    index=False
)
```

The resulting file is:

```text
Processed_Ecommerce_Dataset.csv
```

---

# 🚀 How to Run the Project

## Using Google Colab

1. Open Google Colab.
2. Upload `EcomFusion.ipynb`.
3. Upload the three source CSV files:

   * `Day9_Orders.csv`
   * `Day9_Customers.csv`
   * `Day9_Products.csv`
4. Run the notebook cells sequentially.
5. The processed dataset will be generated as:

```text
Processed_Ecommerce_Dataset.csv
```

---

## Using Jupyter Notebook Locally

Install the required libraries:

```bash
pip install pandas numpy jupyter
```

Launch Jupyter:

```bash
jupyter notebook
```

Open `EcomFusion.ipynb` and execute the cells.

---

# 💡 Applications

The processed dataset can be used for further analysis such as:

* E-commerce sales analysis
* Customer behavior analysis
* Product performance analysis
* Regional sales analysis
* Membership analysis
* Monthly sales trends
* Order-status analysis
* Payment-method analysis
* Data visualization
* Exploratory Data Analysis
* Machine Learning

---

# 🔮 Future Improvements

The project can be extended with:

* Sales visualization using Matplotlib or Seaborn
* Interactive dashboards using Streamlit
* Customer segmentation
* Product performance ranking
* Monthly and regional sales analysis
* Sales forecasting
* Customer lifetime value analysis
* Recommendation systems
* Automated data-quality reports
* Interactive business intelligence dashboards

---

# 📚 Key Pandas Functions Demonstrated

| Function            | Purpose                  |
| ------------------- | ------------------------ |
| `pd.read_csv()`     | Load CSV datasets        |
| `head()`            | View first records       |
| `shape`             | Get dataset dimensions   |
| `columns`           | View feature names       |
| `dtypes`            | Check data types         |
| `isnull()`          | Detect missing values    |
| `pd.to_datetime()`  | Convert dates            |
| `merge()`           | Combine related datasets |
| `concat()`          | Combine DataFrames       |
| `apply()`           | Transform/create columns |
| `.dt.year`          | Extract year             |
| `.dt.month`         | Extract month            |
| `.dt.day`           | Extract day              |
| `.dt.day_name()`    | Extract weekday          |
| `drop_duplicates()` | Remove duplicates        |
| `to_csv()`          | Export processed data    |

---

# 🎓 Learning Outcomes

Through this project, the following data-processing concepts are demonstrated:

* Working with multiple datasets.
* Understanding relationships between datasets.
* Performing relational data merging.
* Combining DataFrames using concatenation.
* Creating derived features.
* Working with DateTime data.
* Applying custom functions to DataFrame columns.
* Performing basic data cleaning.
* Structuring a dataset for further analysis.
* Exporting processed data for future use.

---

## 📄 License

This project is created for **educational and academic purposes**.

---

## ⭐ Project Summary

**EcomFusion** demonstrates a complete basic e-commerce data-processing workflow using Python and Pandas. By integrating **Orders, Customers, and Products** into one structured dataset and enriching it with derived features such as date components, quantity categories, and total order amounts, the project creates a clean foundation for subsequent **data analysis, visualization, and machine learning**.
