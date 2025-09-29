# Python_Bank-segmentation


In this project, I set out to explore bank customer data from [Kaggle](https://www.kaggle.com/datasets/marusagar/bank-customer-attrition-insights) and uncover hidden patterns using clustering techniques. By applying Principal Component Analysis (PCA) and K-Means, I was able to reduce the complexity of the [dataset](/Bank-Customer-Attrition-Insights-Data.xlsx) and group customers into meaningful segments. Take a look at the documentation on Jupyter notebook [here](/kmeans-bank.ipynb). 

## The Process
### 1. Import the Data
On Jupyter notebook, the Bank Segmentation dataset is imported using

     
     df = pd.read_excel("Bank-Customer-Attrition-Insights-Data.xlsx")
     df2 = df[['CreditScore','Balance','Age']]
     
     
Specifically, the choosen columns or dimensions are 'CreditScore', 'Balance', 'Age' so the `df2` is stored with only 3 choosen variables. Next step is plotting the         correlation between each dimensions using heatmap.

<img width="486" height="375" alt="image" src="https://github.com/user-attachments/assets/16154f41-a1cd-4749-936d-97b1ae487a46" />
     
Based on the heatmap, all three dimensions are nearly independent. There are only a little correlations between each 3 dimensions and none of the variable strongly         explain each other, so **the segmentation will rely on their individual contributions**.

   
### 2. Pre process data using PCA
The next step is standarize the feature using z-score normalization `X = StandardScaler().fit_transform(X)` which will rescales all features to mean = 0 and standard deviation = 1, so each variable contributes equally to the PCA calculation. Then, the Principal Component Analysis (PCA) is performed to reduce the 3 variebles into 2 dimensions.


```
Explained variation per principal component: [0.34280164 0.33388278]
Cumulative variance explained by 2 principal components: 67.67%
```


Resulting the 2 components is capture 67.67% from 3 reduced variables, which is acceptable. The contribution each variable for each principal component (PC) as shown below,

```
As per PC 1:
 Balance    0.711186
Age        0.700510
Name: PC_1, dtype: float64

As per PC 2:
 CreditScore    0.970335
Name: PC_2, dtype: float64
```


For PC1 the most contributing variable were 'Balance' and 'Age', and for PC2 the most contributing variable were 'CreditScore'. It can be interpreted as PC1 were combination of Balance and Age, meanwhile PC2 were mostly explained by CreditScore. Hence, the clusters builded later are formed mainly based on Balance + Age vs CreditScore differences.


### 3. Silhoutte score
The silhoutte score is measuring how well the data points fit within their assigned clusters in a clustering algorithm (like KMeans). It is use to choose the optimal number of clusters, the highest score is the best number. 


<img width="567" height="432" alt="image" src="https://github.com/user-attachments/assets/3373e6a5-cc22-4a3e-86f7-f4fbcf0ae32b" />

Optimal number of cluster is 2, which scores the highest near 0.8.



### 4. K-Means analysis
The K-Means algorithm is performed. What K-Means do is grouping data into *k* clusters by minimizing the distance between data points and their cluster’s center (centroid) which is calculated by squared Euclidean distance. There are two clusters, so there is two centroids. The data used is the dataset after standarize and applying PCA.


```
kmeans = KMeans(n_clusters=n1, init='k-means++', random_state=1) #n1=2
y_kmeans = kmeans.fit_predict(pca_2)
```



### 5. The Result
All the process, resulting on two cluster or segmentation of Bank's customer based on customer's Balance, Credit score, and Age. The first cluster (red), around positive values of Principal Component 1 (balance and age). Customer on cluster 1 were older customers with higher account balance because of higher PC1 values. This cluster can be labeled as "Wealthy senior" and customers in this cluster likely more financially stable, higher value customers, could be more loyal but less likely to adopt new products unless they see clear value. 


The second cluster (cyan), around negative values of Principal Component 1 (balance and age). Customer on cluster 2 were younger customers with lower account balance because of lower PC1. This cluster can be labeled as "Emerging Young Customers" as they were likely early in their financial journey, smaller spending power now, but more open to digital products, cross-selling, or upselling as they grow financially. 


<img width="851" height="698" alt="image" src="https://github.com/user-attachments/assets/2ae4ad81-2706-4fde-afbe-011e75b8fbc1" />



```
      CustomerId  Cluster
0       15598695        1
1       15649354        0
2       15737556        0
3       15671610        1
4       15625092        0
...          ...      ...
9995    15583480        1
9996    15620341        1
9997    15613886        1
9998    15792916        0
9999    15710408        0

[10000 rows x 2 columns]
```


Conclusion: This clustering is segment customers by life stage & financial capacity. The result of this clustering can be use to design different marketing, product offerings, and retention strategies for each.
