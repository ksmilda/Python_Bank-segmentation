# Python_Bank-segmentation


In this project, I set out to explore bank customer data from [Kaggle](https://www.kaggle.com/datasets/marusagar/bank-customer-attrition-insights) and uncover hidden patterns using clustering techniques. By applying Principal Component Analysis (PCA) and K-Means, I was able to reduce the complexity of the [dataset](/Bank-Customer-Attrition-Insights-Data.xlsx) and group customers into meaningful segments. Take a look at the documentation on Jupyter notebook [here](/kmeans-bank.ipynb). 

## The Process
### 1. Import the Data
On Jupyter notebook, the Bank Segmentation dataset is imported using

     ```
     df = pd.read_excel("Bank-Customer-Attrition-Insights-Data.xlsx")
     df2 = df[['CreditScore','Balance','Age']]
     ```
     
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



### 5. K-Means analysis
### 6. The Result
