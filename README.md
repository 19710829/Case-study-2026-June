# Case-study-2026-June
This is a case study to practise my data analysis skills. Data are sourced from public old dataset for phenotype profiling. https://www.kaggle.com/competitions/lish-moa/data. 

*** The meaning biological questions to answer: 
1)	The potent cytoxic candidate: screen the top 10 most cytotoxic drugs and define their MoA.
Strategy: a) work at train_feature;
          b) SQL group and display drugs produced highest cytotoxicity;
          c) Genemapping of these treatment to seek signal pathway. 

2)	The potential new drug target: pinpoint those drugs specifically hit at some signal pathway without killing cells?
 Strategy: a) work at train_feature;
          b) SQL group and display drugs produced low to no cytotoxicity;
          c) Use PCA or UMAP to show how "Control" samples (DMSO) cluster differently from "Active" drugs.

20260614 Import dataset
MySQL workbench/Query/ CREATE DATABASE moa_project;
Refresh ALL
Table/Table Data Import Wizard/  Import Train_Drug (column is smaller than 870)
Refresh ALL

Load all datasets into Jupiter Notebook under MoA case study 20260614

Download Jupyter Notebook from ANACONDA
Connect MySQL with Jupyter and Python

import pandas as pd
from sqlalchemy import create_engine

# 1. Load the data into Python memory
df = pd.read_csv('train_features.csv')

# 2. CREATE A FRESH CONNECTION
# RE-TYPE your working password here carefully
my_real_password = 'My_New_Working_Password' 

engine = create_engine(f'mysql+mysqlconnector://root:{my_real_password}@localhost/moa_project')

# 3. PUSH TO SQL
try:
    print("Uploading... this will take about 1-2 minutes...")
    df.to_sql('train_features', con=engine, index=False, if_exists='replace', chunksize=1000)
    print("Success! Table 'train_features' is now in MySQL Workbench.")
except Exception as e:
    print(f"Error: {e}")


In Python:

# 1. Calculate the average of all 'c-' columns in Python
# .filter(regex='^c-') finds all columns starting with 'c-'
df['viability_avg'] = df.filter(regex='^c-').mean(axis=1)

# 2. Update your SQL table with this new 'viability_avg' column
# This makes it easy to query later in Workbench
df.to_sql('train_features_with_avg', con=engine, if_exists='replace', index=False)

print("Average calculated and new table created!")

In SQL:

SELECT sig_id, cp_type, cp_time, viability_avg
FROM train_features_with_avg
WHERE cp_type = 'cp_samples' -- Ignore the control (DMSO) samples
ORDER BY viability_avg ASC     -- Most negative (most toxic) first
LIMIT 10;

Only title showed without value.

Troubleshooting: 
In Python
print(f"Total rows in Python memory: {len(df)}")
print(f"Number of columns: {len(df.columns)}")
print("First 5 averages calculated:")
print(df[['sig_id', 'viability_avg']].head())

# 1. Ensure the engine is fresh
engine = create_engine(f'mysql+mysqlconnector://root:Haojun0829@localhost/moa_project')

# 2. Re-upload with a smaller chunksize
try:
    print("Starting upload... please wait for the Success message.")
    # We use method='multi' and chunksize to be very safe
    df.to_sql('train_features_with_avg', 
              con=engine, 
              if_exists='replace', 
              index=False, 
              chunksize=500) 
    print("Success! Data has been successfully moved to MySQL.")
except Exception as e:
    print(f"Upload failed: {e}")

    In SQL:

SELECT sig_id, viability_avg 
FROM train_features_with_avg 
WHERE cp_type = 'cp_samples'
ORDER BY viability_avg ASC 
LIMIT 10;;

top 10 Most cytotoxicity candidate drugs listed.  Export the CSV file into my laptop.

In Python 
# Save my code in Python

# 1. Pull the top 10 from SQL into a new Python variable
query = "SELECT * FROM v_top_10_cytotoxic"
df_top_10 = pd.read_sql(query, con=engine)

# 2. Save it as a local CSV for your GitHub
df_top_10.to_csv('top_10_cytotoxic_summary.csv', index=False)

print("Top 10 results saved as a CSV file on your laptop!")
