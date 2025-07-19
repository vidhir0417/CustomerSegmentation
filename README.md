# Machine Learning II - Customer Segmentation

This document focuses on "Clustering and Customer Segmentation" for a supermarket and aims to identify distinct customer groups based on shared characteristics using unsupervised machine learning techniques.

## Purpose

The main purpose of this project is to:
* Divide a large customer base into smaller, homogeneous groups.
* Understand customer desires and behavior patterns.
* Develop targeted promotional strategies to increase supermarket profit.
* Provide valuable insights for improving business strategies.

## Project Phases & Methodology

The project followed a comprehensive approach, which was encompassing data preprocessing, customer segmentation and targeted promotion generation.

### 1. Exploratory Data Analysis and Preprocessing
* **Data Loading & Visualization:** Initial loading of customer information and visualization of customer locations using a map.
* **Variable Renaming:** Removed redundant words (e.g., "lifetime") from variable names for clarity.
* **Missing Value Handling:** Identified and imputed missing values using KNN Imputer (for variables like `typical_hour`, `distinct_stores_visited`, `spend_fish`, `teens_home`, `spend_vegetables`, `number_complaints`, `kids_home`). `loyalty_card_number` was dropped due to irrelevance.
* **Outlier Treatment:** Identified outliers in almost all variables. Treated a subset of outliers (summing to ~7333) to avoid significant data loss.
* **Variable Transformations:**
    * Created `customer_graduate` column from `customer_name`.
    * Converted `customer_birthdate` to datetime and extracted the year.
    * Converted numeric variables to integers.
    * Removed `latitude` and `longitude` after initial mapping.
* **Feature Selection & Correlation Matrix:** Initially aimed for feature selection based on correlation but decided to keep all variables due to poor clustering results when removing them. The correlation matrix was still used for analysis.
* **Scaling:** Applied `RobustScaler` to the data, as it performed best compared to `MinMaxScaler` and `StandardScaler` due to the presence of untreated outliers.

### 2. Customer Segmentation and Clustering
* **Clustering Algorithms Implemented:**
    * **DBSCAN:** Used primarily for identifying multidimensional outliers. Hyperparameters (`min_samples`, `eps`) were tuned through trial and error (e.g., `min_samples=12`). Achieved a silhouette score of ~0.29, indicating low cluster quality but useful for outlier detection.
    * **Tandem Approach 1: K-Means + Hierarchical Clustering:** K-Means was used first, followed by Agglomerative Clustering. Chosen final number of clusters was **10**, based on dendrogram analysis.
    * **Tandem Approach 2: SOM (Self-Organizing Maps) + Hierarchical Clustering:** SOM was implemented first, followed by Agglomerative Clustering. Also resulted in **10 clusters**.
* **Final Cluster Determination:** A confusion matrix was used to compare the agreement between K-Means and SOM results. Another Hierarchical Clustering was performed on the means of the other variables based on these initial clusters.
* **Refining Clusters:** Initially, 9 clusters were chosen. Due to a large, unseparated "Cluster 0" in UMAP visualizations, it was further broken down into 5 sub-clusters, resulting in a total of **13 final clusters**.
* **Cluster Naming:** Customized names were assigned to the 10 main clusters based on their characteristics (Fake Vegetarians, Millennials, Gamer Parents, Misers, Local Folks, Youngsters, Balanced Parents, Settled Techies, Stingy Pescatarians, Retired Elders).

### 3. Targeted Promotions (Association Rules)
* **Data Integration:** Joined the final cluster assignments with customer transaction data (`cust_basket`).
* **Cluster-Specific Analysis:** Filtered transaction data by cluster to generate customized promotions.
* **Apriori Algorithm:** Applied the Apriori algorithm and `association_rules` to identify frequent itemsets and strong association rules (based on support, confidence and lift values).
* **Promotions Suggested:** Specific promotions were tailored for each identified cluster (e.g., "Soup Lovers" for Fake Vegetarians, "Overenergize" for Millennials, "Grape Expectations" for Misers and more).

## Conclusion

The project successfully implemented unsupervised machine learning techniques for customer segmentation, identifying distinct customer groups within the supermarket's data. By understanding these segments and their purchasing behaviors, the supermarket can develop targeted promotions and strategies to increase profitability and better cater to client desires.
