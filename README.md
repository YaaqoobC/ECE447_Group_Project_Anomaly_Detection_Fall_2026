# ECE447 Group Project Anomaly Detection Fall 2026 - Credit Card Fraud Detection
## Members:
1. Yaaqoob Choulli - yaaqoob
2. Liam Reschke - lreschke
3. Tairan Xi - tairan1

## Problem Framing (%15)
1. Clear definition of anomaly:

   Given our dataset, credit card fraud detection, the definition of an anomily is a fraudulent credit card transaction. In other words it is a value of 1 in our `Class` feature. Taking a deeper dive, our data has been 'preprocessed' already via a Principal Component Analysis (PCA) to anonymized prior confidential data. This means that our columns are composed of 28 'v' colums (ie. v1, v2, ..., v28), a time column, an ammount column, and a class column.
   - `vX`: 28 numerical anonymized features
   - `Time`: The number of seconds between each transaction and the first transaction in the dataset
   - `Amount`: The transaction amount
   - `Class`: (Label, ie. ground truth) 0 if the transaction is genuine and 1 if it is fraudulent.

3. Supervised vs. unsupervised distinction:

    
4. Justification of approach:

