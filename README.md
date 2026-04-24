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


## Part 5 Anomaly visualization:
### 5.1:
Lets begin by observing the first plot, the ground truth. The first thing we can see is that there are 98 fraud cases that are either clustered near the middle left or scattered around the plot. This tells us our earlier intuition is correct, fraud is diverse, there is teh common fraud 'neighborhood' however there are fraud outliers across the entire board. If we look at the genuine transactions in blue there is no clear pattern but instead an almost scattered cloud. 

Lets now look at the second plot, the `Mahalanobis Distance` (top right). Overall it caught 70 of the 98 fraud cases however what is interesting is that of the 28 misses, almost all of them are ones located in the main cluster of points. MD works by measuring distance from the global mean, with this being said, fraud that happens to look average do not trigger as easily. This is most likely the reason for the performance dip around the common fraud neighborhood as opposed to ones on the outskirts. Following this point we see that most of the FPs (wrongly flagged transactions) are on the outer fringes of the genuine 'cloud'. It is important to note that `MD` preforms the best when it comes to FP, it beats the `KNN` by over double, and `IS` by about 7 times.

Our next plot is the `k-Nearest Neighbors` with (k=5), this has the best balance. It caught 81 of the 98 while keeping false positives at 73. The FP are slightly worse than the `MD` but our TP is slightly better. Looking at the location of missed fraud cases, they are all more embedded and surrounded by genuine points. Nothing was missed in the fraud neighborhood (no purple in the red cluster) but instead around the middle of the genuine 'cloud'. This is one of `KNN`s weakness, the algorithm looks locally, if your are surrounded by genuine you look genuine. Analyzing the orange points (FPs), we see they are spread around the outer edges, these are genuine transactions that happen to be in sparse neighborhoods, so their nearest neighbors are far away, making them look anomalous.

Our final plot, the `Isolation Forest` (bottom right) is the most interesting. Catching 88 of the 98 fraudulent transactions, it is the best recall by far. The caveat here is the astonishingly high FPs (wrongly flagged) points, 209. The orange dots are scattered everywhere not just the edges nor just the center. This paints a clear story, the `IF` is aggressive, it seems to isolate anything looking slight unusual anywhere in the space. In other words we can catch more fraud but we sweep up a lot of innocent transactions. 

If we do a cross model comparison we see that all three models successfully catch the majority if not all of the fraud cases that lie in the bottom left cluster. These can be considered easy given they are geographically separated from the genuine transactions in the feature space. This means the optimal model is derived from the performance of the scattered isolated fraud points. From a Precision Recall standpoint, MD has high precision and low recall while KNN has a good middle ground, and IS has high recall and low precision. One final shared observation we can make is that, all three models miss fraud cases that sit deep in the genuine cloud, these are obviously the hardest cases given they look the most genuine.
