Heart Failure Prediction Analysis This repository contains a Jupyter Notebook (heart-failure-ipynb.ipynb) that performs an exploratory data analysis (EDA) on the Heart Failure Prediction. The analysis focuses on understanding the dataset's structure, checking for missing values, detecting outliers, and visualizing numerical features to gain insights into the data. Future work includes training machine learning models and identifying important features for heart disease prediction. Overview The notebook uses Python and popular data analysis libraries to explore the Heart Failure Prediction dataset, sourced from Kaggle. The dataset contains clinical records of patients to predict the likelihood of heart disease. The analysis includes loading the data, inspecting its structure, checking for data quality issues, identifying outliers, and visualizing numerical features using box plots. Dataset Description The dataset (heart.csv) contains 918 patient records with 12 features related to clinical measurements and heart disease outcomes. The features are:

Age: Patient's age (integer, 28 to 77 years). Sex: Patient's sex (object, M/F). ChestPainType: Type of chest pain (object, ATA/NAP/ASY/TA). RestingBP: Resting blood pressure in mmHg (integer, 0 to 200). Cholesterol: Serum cholesterol in mg/dl (integer, 0 to 603). FastingBS: Fasting blood sugar > 120 mg/dl (integer, 0/1). RestingECG: Resting electrocardiographic results (object, Normal/ST/LVH). MaxHR: Maximum heart rate achieved (integer, 60 to 202). ExerciseAngina: Exercise-induced angina (object, Y/N). Oldpeak: ST depression induced by exercise relative to rest (float, -2.6 to 6.2). ST_Slope: Slope of the peak exercise ST segment (object, Up/Flat/Down). HeartDisease: Presence of heart disease (integer, 0/1).

Key Statistics

Total Records: 918 Numerical Columns: 7 (Age, RestingBP, Cholesterol, FastingBS, MaxHR, Oldpeak, HeartDisease) Categorical Columns: 5 (Sex, ChestPainType, RestingECG, ExerciseAngina, ST_Slope) Mean Age: 53.51 years (std: 9.43) Mean RestingBP: 132.40 mmHg (std: 18.51) Mean Cholesterol: 198.80 mg/dl (std: 109.38) Mean MaxHR: 136.81 (std: 25.46) Mean Oldpeak: 0.89 (std: 1.07) HeartDisease Prevalence: 55.34% (508 patients with heart disease, 410 without)

Analysis Steps The notebook performs the following steps:

Environment Setup:

Imports essential libraries: numpy for numerical operations, pandas for data manipulation, and matplotlib.pyplot for visualization. Lists files in the input directory (/kaggle/input/) to locate heart.csv.

Data Loading:

Loads the dataset into a Pandas DataFrame using pd.read_csv("/kaggle/input/heart-failure-prediction/heart.csv").

Data Inspection:

Uses df.info() to display data types and check for missing values. Uses df.describe() to provide summary statistics for numerical columns. Displays the first five rows with df.head() to preview the data structure.

Missing Values Check:

Separates numerical and categorical columns using select_dtypes. Confirms no missing values in either numerical or categorical columns.

Outlier Detection:

Defines a function detect_outliers_iqr to identify outliers using the Interquartile Range (IQR) method for numerical columns. Outliers are detected based on values outside 1.5 * IQR from Q1 and Q3. Results: Age: No outliers. RestingBP: 28 outliers (e.g., 0, 172–200 mmHg). Cholesterol: 183 outliers (e.g., 0, 409–603 mg/dl). FastingBS: 214 outliers (all values of 1, indicating fasting blood sugar > 120 mg/dl). MaxHR: 2 outliers (60, 63). Oldpeak: 16 outliers (e.g., -2.6, 3.8–6.2). HeartDisease: No outliers (binary variable).

Visualization:

Creates a box plot for numerical columns using matplotlib to visualize outliers. The plot highlights the spread and potential outliers in features like Cholesterol, RestingBP, and Oldpeak.

Data Export:

Saves the DataFrame to cleaned_data.csv without modifications, preserving the original dataset for further analysis.

Key Findings

Data Quality: The dataset is complete with no missing values across all 918 records and 12 features. Outliers: Significant outliers in Cholesterol (183 instances, including unrealistic values like 0 mg/dl) and RestingBP (e.g., 0 mmHg) suggest potential data entry errors or special cases. FastingBS outliers (214 instances of 1) reflect the binary nature of the variable rather than anomalies. Oldpeak and MaxHR have fewer outliers, but extreme values may warrant further investigation.

Feature Distribution: Numerical features like Age and MaxHR are relatively normally distributed, while Cholesterol and RestingBP show wider variability. The box plot visualization highlights the presence of extreme values, particularly in Cholesterol and RestingBP.

Heart Disease Distribution: Approximately 55% of patients have heart disease (HeartDisease = 1), indicating a balanced dataset for predictive modeling.

Machine Learning Model Training

Splitting the dataset into training and testing sets. Encoding categorical variables (e.g., using one-hot encoding for Sex, ChestPainType, etc.). Training models such as Logistic Regression, Random Forest, or Gradient Boosting to predict HeartDisease. Evaluating model performance using metrics like accuracy, precision, recall, F1-score, and ROC-AUC.

Feature Importance Note: Feature importance analysis is not included in the current notebook. Planned steps include:

Using models like Random Forest or XGBoost SVC or Logistic Regression to compute feature importance scores. Visualizing feature importance with bar plots to identify key predictors of heart disease (e.g., Oldpeak, ST_Slope, ChestPainType). Conducting correlation analysis to explore relationships between features and HeartDisease.

Dependencies The notebook requires the following Python libraries:

numpy (for numerical operations) pandas (for data manipulation and analysis) matplotlib (for visualization)

To install dependencies: pip install numpy pandas matplotlib

How to Run

Set Up Environment:

Ensure Python 3.10 or later is installed. Install dependencies using the command above.

Download the Dataset:

Obtain heart.csv from Kaggle Heart Failure Prediction Dataset. Place heart.csv in the /kaggle/input/heart-failure-prediction/ directory or update the file path in the notebook.

Run the Notebook:

Open heart-failure-ipynb.ipynb in Jupyter Notebook or JupyterLab. Execute cells sequentially to perform the analysis. The notebook generates a box plot and saves the dataset as cleaned_data.csv in the working directory.

Future Work

Data Cleaning: Address outliers in Cholesterol and RestingBP (e.g., replace or remove unrealistic values like 0). Feature Engineering: Encode categorical variables for machine learning. Model Development: Implement and compare ML models for heart disease prediction. Feature Importance Analysis: Identify and visualize key features influencing heart disease. Further Visualizations: Explore correlations between features and HeartDisease using heatmaps or pair plots.

License This project is licensed under the MIT License. See the LICENSE file for details. Dataset Source The dataset is sourced from Kaggle: Heart Failure Prediction Dataset.
