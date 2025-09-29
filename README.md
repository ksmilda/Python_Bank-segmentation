# Python_Bank-segmentation


In this project, I set out to explore bank customer data from [Kaggle](https://www.kaggle.com/datasets/marusagar/bank-customer-attrition-insights) and uncover hidden patterns using clustering techniques. By applying Principal Component Analysis (PCA) and K-Means, I was able to reduce the complexity of the [dataset](/Bank-Customer-Attrition-Insights-Data.xlsx) and group customers into meaningful segments. Take a look at the documentation on Jupyter notebook [here](/kmeans-bank.ipynb). Click this link to straight to the result of segmentation.

## The Process
1. Import the Data
     On Jupyter notebook, the Bank Segmentation dataset is imported using
     ```
     df = pd.read_excel("Bank-Customer-Attrition-Insights-Data.xlsx")
     df2 = df[['CreditScore','Balance','Age']]
     ```
     Specifically, the choosen columns or dimensions are 'CreditScore', 'Balance', 'Age' so the `df2` is stored with only 3 choosen dimensions. Next step is plotting the         correlation between each dimensions using heatmap.


     <img width="486" height="375" alt="image" src="https://github.com/user-attachments/assets/16154f41-a1cd-4749-936d-97b1ae487a46" />

    Based on the heatmap, all three dimensions are nearly independent. There are only a little correlations between each 3 dimensions and none of the dimensions strongly         explain each other, so **the segmentation will rely on their individual contributions**.

   
2. Pre process data using PCA
     The next step is standarize the feature using 
4. Sillhoute score
5. K-Means analysis
6. The Result
