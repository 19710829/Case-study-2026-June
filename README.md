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
