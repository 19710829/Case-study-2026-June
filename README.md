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
