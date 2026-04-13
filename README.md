# ECE447 Group Project Anomaly Detection Fall 2026 - Credit Card Fraud Detection
## Members:
1. Yaaqoob Choulli - yaaqoob
2. Liam Reschke - lreschke
3. Tairan Xi - tairan1

## Problem Framing (%15)
1. Clear definition of anomaly:

   Given our dataset, credit card fraud detection, the definition of an anomily is a fraudulent credit card transaction. In other words it is a value of 1 in our `Class` feature. By doing some quick observation we can see that our of our entire data set a fraudulent case occurs only `0.173%` of the time. Taking a deeper dive, our data has been 'preprocessed' already via a Principal Component Analysis (PCA) to anonymized prior confidential data. This means that our columns are composed of 28 'v' colums (ie. v1, v2, ..., v28), a time column, an ammount column, and a class column:
   - `vX`: 28 numerical anonymized features
   - `Time`: The number of seconds between each transaction and the first transaction in the dataset
   - `Amount`: The transaction amount
   - `Class`: (Label, ie. ground truth) 0 if the transaction is genuine and 1 if it is fraudulent.

2. Supervised vs. unsupervised distinction and Justification of approach:

   For this project we have chose to use ***Unsupervised Learning***.

   The distrinction between the two forms are the following:
   - With **Supervised Learning** the model is trained using both the transaction features and the explicit labels (ie. class). This means learning a specific decision boundary between known historical "Fraud" and known historical "Genuine" transactions.
   - With **Unsupervised Learning** the model is trained purely on the transaction features (ie. v1-v28) without any access to the labels. This means that the model learns the underlying math structure of the data to flag points that greatly deviate from the majority.

   The justification for choosing unsupervised learning is the following;
      - We want to be able to flag a transition that would most likely not be similar to something done historically. In other words, with a supervised learning model we are focused on catching historical forms of fraud, if a modern attach was made, the model would likely not be able to catch it.
     - Algorithms recommended to us like DBSCAN and Isolation Forest are generally designed to find outliers in unlabeled multidimensional space, these are primed for unsupervised learning.
