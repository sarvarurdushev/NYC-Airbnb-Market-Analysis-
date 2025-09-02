# NYC-Airbnb-Market-Analysis-
Welcome to New York City, one of the most-visited cities in the world! This project analyzes the NYC Airbnb market using 2019 data to understand pricing trends, room types, and review patterns across different boroughs and neighborhoods.

# NYC Airbnb Market Analysis 🏙️

Welcome to New York City, one of the most-visited cities in the world! This project analyzes the NYC Airbnb market using 2019 data to understand pricing trends, room types, and review patterns across different boroughs and neighborhoods.

## 📊 Dataset Overview

This analysis combines data from multiple file formats to provide comprehensive insights into NYC's Airbnb market:

### **Data Sources**
- **`data/airbnb_price.csv`** - Pricing and location data
- **`data/airbnb_room_type.xlsx`** - Room types and descriptions  
- **`data/airbnb_last_review.tsv`** - Host information and review dates

### **Data Schema**
| File | Columns | Description |
|------|---------|-------------|
| `airbnb_price.csv` | `listing_id`, `price`, `nbhood_full` | Unique identifier, nightly price in USD, borough and neighborhood |
| `airbnb_room_type.xlsx` | `listing_id`, `description`, `room_type` | Unique identifier, listing description, room type (shared/private/entire home) |
| `airbnb_last_review.tsv` | `listing_id`, `host_name`, `last_review` | Unique identifier, host name, last review date |

## 🔧 Analysis Code

```python
# Import necessary packages
import pandas as pd
import numpy as np

# Import CSV for prices
airbnb_price = pd.read_csv('data/airbnb_price.csv')

# Import Excel file for room types
airbnb_room_type = pd.read_excel('data/airbnb_room_type.xlsx')

# Import TSV for review dates
airbnb_last_review = pd.read_csv('data/airbnb_last_review.tsv', sep='\t')

# Join the three data frames together into one
listings = pd.merge(airbnb_price, airbnb_room_type, on='listing_id')
listings = pd.merge(listings, airbnb_last_review, on='listing_id')

# What are the dates of the earliest and most recent reviews?
# To use a function like max()/min() on last_review date column, it needs to be converted to datetime type
listings['last_review_date'] = pd.to_datetime(listings['last_review'], format='%B %d %Y')
first_reviewed = listings['last_review_date'].min()
last_reviewed = listings['last_review_date'].max()

# How many of the listings are private rooms?
# Since there are differences in capitalization, make capitalization consistent
listings['room_type'] = listings['room_type'].str.lower()
private_room_count = listings[listings['room_type'] == 'private room'].shape[0]

# What is the average listing price?
# To convert price to numeric, remove " dollars" from each value
listings['price_clean'] = listings['price'].str.replace(' dollars', '').astype(float)
avg_price = listings['price_clean'].mean()

# Create summary results
review_dates = pd.DataFrame({
    'first_reviewed': [first_reviewed],
    'last_reviewed': [last_reviewed],
    'nb_private_rooms': [private_room_count],
    'avg_price': [round(avg_price, 2)]
})

print(review_dates)
```

## 📈 Key Insights

The analysis provides answers to several important questions about the NYC Airbnb market:

- **Review Timeline**: Identifies the earliest and most recent review dates to understand the data's temporal scope
- **Room Type Distribution**: Counts private rooms vs. shared rooms and entire homes/apartments
- **Price Analysis**: Calculates average nightly pricing across all listings
- **Geographic Coverage**: Examines listings across NYC's five boroughs

## 🛠️ Technical Highlights

### **Data Integration Challenges Solved**
- **Multiple File Formats**: Successfully merges CSV, Excel (.xlsx), and TSV files
- **Data Cleaning**: Handles inconsistent capitalization in room types
- **Price Formatting**: Converts price strings with " dollars" suffix to numeric values
- **Date Parsing**: Converts date strings to datetime objects for temporal analysis

### **Data Processing Techniques**
- Pandas merging on common keys (`listing_id`)
- String manipulation for data standardization
- Type conversion for numerical and datetime operations
- Aggregation functions for summary statistics

## 📋 Requirements

```python
pandas
numpy
openpyxl  # Required for reading Excel files
```

## 🚀 Getting Started

**1.** Clone this repository
**2.** Install required packages: `pip install pandas numpy openpyxl`
**3.** Ensure data files are in the `data/` directory
**4.** Run the analysis script

## 📊 Expected Output

The script generates a summary DataFrame with:
- **First review date** in the dataset
- **Most recent review date** in the dataset  
- **Total count** of private room listings
- **Average nightly price** across all listings

## 🤝 Contributing

Feel free to fork this project and submit pull requests for additional analysis or improvements to the data processing pipeline.

## 📝 License

This project is available under the MIT License.
