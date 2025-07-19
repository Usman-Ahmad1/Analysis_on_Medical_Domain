# COVID-19 Death Counts Analysis

This repository contains a Jupyter Notebook (`covid19-goals1.ipynb`) that performs an initial analysis of a dataset containing provisional COVID-19 death counts and rates by month, jurisdiction of residence, and demographic characteristics. The notebook focuses on data exploration, preparation for time series analysis, handling missing values, and aggregating data to create a new dataset.

## Dataset
The dataset, sourced from the National Center for Health Statistics (NCHS), contains provisional COVID-19 death counts and rates by month and year of death, jurisdiction of residence (United States and HHS Regions), and demographic characteristics (sex, age, race and Hispanic origin, and age/race and Hispanic origin). The United States data includes the 50 states and the District of Columbia. Key details include:

- **Data Description**: Deaths with confirmed or presumed COVID-19, coded to ICD-10 code U07.1. The dataset represents total deaths received and coded as of the analysis date (07/10/2025) and may not include all deaths for the period due to reporting lags (1-8 weeks or more, varying by jurisdiction).
- **Jurisdictions**: Includes the United States and ten HHS regions:
  - Region 1: Connecticut, Maine, Massachusetts, New Hampshire, Rhode Island, Vermont
  - Region 2: New Jersey, New York
  - Region 3: Delaware, District of Columbia, Maryland, Pennsylvania, Virginia, West Virginia
  - Region 4: Alabama, Florida, Georgia, Kentucky, Mississippi, North Carolina, South Carolina, Tennessee
  - Region 5: Illinois, Indiana, Michigan, Minnesota, Ohio, Wisconsin
  - Region 6: Arkansas, Louisiana, New Mexico, Oklahoma, Texas
  - Region 7: Iowa, Kansas, Missouri, Nebraska
  - Region 8: Colorado, Montana, North Dakota, South Dakota, Utah, Wyoming
  - Region 9: Arizona, California, Hawaii, Nevada
  - Region 10: Alaska, Idaho, Oregon, Washington
- **Data Limitations**: 
  - Death counts before or after the reporting period are excluded.
  - Data timeliness varies by state (daily, weekly, or monthly reporting).
  - Sub-national death counts between 1-9 are suppressed per NCHS confidentiality standards.
  - Rates based on death counts less than 20 are suppressed per NCHS reliability standards.
- **Rate Calculations**: Rates are age-adjusted to the 2000 standard population using the direct method and calculated using 2021 population estimates (Blended Base, as of July 1, 2021) from the US Census Bureau. Annualized rates represent deaths per 100,000 population expected in a year if the observed period-specific rate prevailed.
- **Columns**:
  - `data_as_of`: Date of data update (e.g., 07/10/2025).
  - `jurisdiction_residence`: Geographic region (United States, Region 1-10).
  - `year` and `month`: Time period of the data.
  - `group`: Demographic category (Sex, Age, Race, Race and Age).
  - `subgroup1` and `subgroup2`: Specific demographic groups (e.g., Male/Female, age ranges, racial groups).
  - `COVID_deaths`: Number of COVID-19 deaths.
  - `crude_COVID_rate` and `aa_COVID_rate`: Crude and age-adjusted death rates.
  - `crude_COVID_rate_ann` and `aa_COVID_rate_ann`: Annualized crude and age-adjusted death rates.
  - `footnote`: Notes on data reliability (e.g., "Rates for death counts <20 are unreliable").
  - `date_new`: Derived datetime column for time series analysis.

## Notebook Overview
The `covid19-goals1.ipynb` notebook performs the following tasks:
1. **Environment Setup**: Imports necessary Python libraries for data analysis and visualization.
2. **Data Loading**: Reads the CSV dataset into a Pandas DataFrame.
3. **Data Exploration**: Examines the structure, summary statistics, and missing values in the dataset.
4. **Data Preparation**: Creates a new datetime column (`date_new`) for time series analysis.
5. **Missing Value Imputation**: Addresses missing values in key columns like `COVID_deaths`.
6. **Data Aggregation**: Aggregates and sums data to create a new dataset for analysis.
7. **Preliminary Analysis**: Investigates the distribution of key categorical columns and missing values.

## Methods Used
The notebook employs the following Python libraries and methods:

### Libraries
- **Pandas**: Used for data manipulation, loading the CSV file, and creating a DataFrame.
  - `pd.read_csv()`: Loads the dataset into a DataFrame.
  - `pd.to_datetime()`: Converts year and month columns into a datetime format for time series analysis.
  - `df.head()`: Displays the first few rows of the DataFrame.
  - `df.info()`: Provides an overview of the DataFrame's structure, including data types and non-null counts.
  - `df.describe()`: Generates summary statistics for numerical columns.
  - `df.value_counts()`: Counts unique values in categorical columns (`jurisdiction_residence`, `group`, `subgroup1`).
  - `df.isna().sum()`: Identifies missing values in specified columns (`COVID_deaths`, `crude_COVID_rate`).
  - `df[df['COVID_deaths'].isna()]`: Filters rows where `COVID_deaths` is missing to inspect related columns.
  - `df.fillna()`: Used to impute missing values (e.g., filling missing `COVID_deaths` with 0 or mean values).
  - `df.groupby()` and `sum()`: Aggregates data by specific columns (e.g., `date_new`, `jurisdiction_residence`, or `group`) and sums `COVID_deaths` to create a new dataset.
- **NumPy**: Imported for numerical operations, such as computing mean values for imputation.
- **Matplotlib and Seaborn**: Imported for visualization purposes, though no plots are generated in this notebook.
- **OS**: Used to list files in the input directory to verify the dataset's location.

### Data Manipulation
- **Datetime Conversion**: A new column, `date_new`, is created by combining `year` and `month` columns into a datetime format using `pd.to_datetime(df['year']*100 + df['month'], format='%Y%m')`. This prepares the dataset for time series analysis.
- **Index Modification**: The DataFrame's index is set to `date_new` to facilitate time-based analysis.
- **Missing Value Imputation**: Missing values in `COVID_deaths` (13,852 missing) and other columns like `crude_COVID_rate` (18,539 missing) are handled by:
  - Filling missing `COVID_deaths` with 0, assuming no deaths occurred in those cases (common for low-count scenarios, especially given NCHS suppression rules for counts 1-9).
  - Optionally, imputing with the mean or median of `COVID_deaths` for specific groups or jurisdictions to preserve data trends.
- **Data Aggregation**: A new dataset is created by aggregating `COVID_deaths` using `df.groupby()` on columns like `date_new`, `jurisdiction_residence`, or `group`, followed by `sum()` to compute total deaths. This results in a condensed dataset suitable for time series or comparative analysis.

### Exploratory Data Analysis
- **Dataset Structure**: `df.info()` is used to inspect the DataFrame's columns, data types, and missing values.
- **Summary Statistics**: `df.describe()` provides statistical insights into numerical columns like `COVID_deaths`, `crude_COVID_rate`, and `aa_COVID_rate`.
- **Categorical Analysis**: `value_counts()` is applied to `jurisdiction_residence`, `group`, and `subgroup1` to understand the distribution of categorical variables.
- **Missing Value Analysis**: Missing values in `COVID_deaths` and `crude_COVID_rate` are quantified, and rows with missing `COVID_deaths` are inspected to identify patterns, likely due to suppression for low counts or incomplete reporting.

## Key Observations
- The dataset contains 58,806 entries across 14 columns (after adding `date_new`).
- Significant missing values are present in columns like `subgroup2` (13,068 missing), `COVID_deaths` (13,852 missing), `crude_COVID_rate` (18,539 missing), `aa_COVID_rate` (54,100 missing), and `footnote` (40,267 missing), likely due to NCHS suppression rules or reporting lags.
- The dataset is grouped by demographic categories (`Sex`, `Age`, `Race`, `Race and Age`), with `subgroup1` and `subgroup2` providing further granularity.
- The `date_new` column enables time series analysis by converting `year` and `month` into a datetime format.
- Imputation with zeros for `COVID_deaths` aligns with NCHS suppression for counts 1-9, while mean/median imputation may be used for specific groups to maintain data integrity.
- Aggregated data (e.g., total `COVID_deaths` by `date_new` or `jurisdiction_residence`) forms a new dataset for streamlined analysis.

## Usage
To run the notebook:
1. Ensure you have Python 3.11.13 installed, along with the required libraries (`pandas`, `numpy`, `matplotlib`, `seaborn`).
2. Place the dataset CSV file in the appropriate directory (e.g., `/kaggle/input/` as specified in the notebook).
3. Open the notebook in Jupyter or a compatible environment (e.g., Kaggle, Google Colab).
4. Execute the cells sequentially to load, explore, impute, aggregate, and prepare the data.

## Future Steps
- Enhance missing value imputation by testing different strategies (e.g., interpolation, group-specific means) while considering NCHS suppression rules.
- Perform time series analysis using the `date_new` column to identify trends in COVID-19 deaths over time.
- Create visualizations (e.g., line plots for deaths over time, bar charts for deaths by jurisdiction or demographic group) using Matplotlib and Seaborn.
- Analyze the aggregated dataset to compare death counts and rates across jurisdictions or demographic groups, noting that direct comparisons across jurisdictions are cautioned due to varying reporting timeliness.
- Save the aggregated dataset as a new CSV file for further analysis.
  
- **Current work on  Temporal Trends in COVID-19 Mortality in this data set**:
  - **Monthly/Quarterly Patterns in Death Counts and Rates**: Analyze `COVID_deaths`, `crude_COVID_rate`, and `aa_COVID_rate` over `date_new` to identify monthly or quarterly trends, using techniques like rolling averages or grouping by quarter.
  - **Identification of Pandemic Waves/Peaks**: Detect periods of high mortality (pandemic waves) by analyzing peaks in `COVID_deaths` or rates over time, potentially using peak detection algorithms or visual inspection of time series plots.
  - **Seasonality Analysis**: Investigate seasonal patterns in COVID-19 mortality by decomposing time series data into trend, seasonal, and residual components using methods like seasonal decomposition (`statsmodels.tsa.seasonal_decompose`) or Fourier analysis.

## Requirements
- Python 3.11.13
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Statsmodels (for seasonality analysis)

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

## Acknowledgments
- Dataset provided by the National Center for Health Statistics (NCHS).
- Population estimates based on the US Census Bureau’s Blended Base (July 1, 2021).
- Rate calculation methodology referenced from CDC’s NCHS Data Presentation Standards for Proportions and NVSR reports.
