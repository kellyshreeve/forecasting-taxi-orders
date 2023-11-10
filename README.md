## Time Series Taxi Driver Predictions

<p align="center">
  <img src="images/taxi_clipart.png"
  width="350"
  height="250"
  alt="Phone clip art">
</p>

### Table of Contents

1. [Project Overview](#overview)  
2. [Installation and Setup](#setup)  
3. [Data](#data)  
4. [Code Structure](#structure)  
5. [Results and Evaluation](#results)  
6. [Conclusions and Business Recommendations](#conclusions)  

### 1. Project Overview<a id='overview'></a>
**Background:** Sweet Lift Taxi company has historical data on taxi orders over time and wants to predict the number of taxi orders expected in future hours.  

**Purpose:** Build a time series model that minimizes RMSE when predicting the number of taxi orders in the next hour.  

**Techniques:** Seasonal decomposition, tuning time series linear regression and random forest regression with skforecast.  

### 2. Installation and Setup<a id='setup'></a>

#### Codes and Resources Used

  - **Editor Used**: Visual Studio Code
  - **Python Version**: 3.10.9

#### Python Packages Used

  - **General Purpose**: ```numpy, time```  
  - **Data Manipulation**: ```pandas```  
  - **Data Visualization**: ```seaborn```  
  - **Machine Learning**: ```sklearn```  
  - **Time Series Modeling**: ```statsmodels, skforecast```

### 3. Data<a id='data'></a>

*taxi.csv* 

**Target**:   
* *num_orders*: taxi rides ordered per hour

**Features**:   
* *rolling_mean*: rolling mean of prior 24 hours
* *year*: year of taxi order
* *month*: month of taxi order
* *day*: day of taxi order
* *dayofweek*: day of week of taxi order
* *hour*: hour of day
 
#### Data Acquisition

The data were provided by TripleTen's Data Science bootcamp. The full dataset is loaded into the notebook but is proprietary information and cannot be shared online.

#### Data Preprocessing

Data were checked for missing values and duplicates. None were found.

Six additional features were created:  
1. rolling_mean for 24 hours prior
2. year
3. month
4. day
5. day of week
6. hour

### 4. Code Structure<a id='structure'></a>
```
  ├── LICENSE
  ├── README.md          
  │
  ├── images
  │   └── churn_over_time.png
  │   └── class_imbalance.png 
  │   └── correlation_heatmap.png 
  │   └── histograms.png 
  │   └── test_results.png 
  │   └── training_results.png     
  │
  └── notebooks  
      └── taxi_analysis.ipynb
```

### 5. Results and Evaluation<a id='results'></a>

#### Exploratory Analysis
 
<p align="center">
  <img src="/images/orders_by_hour.png"
  width="400"
  height="300"
  alt="Time series orders per hour">
</p>

There are fewer customers who churned than did not churn. This is an imbalanced classification problem. Class balancing and weighting techniques will be applied.

<p align="center">
  <img src="/images/residuals.png" 
  width="650"
  height="300"
  alt="seasonality and residuals">
</p>

Customers who began their contracts in 2014 - 2018 are almost all still with the company.  
About 50% of customers who began their contracts in 2019 - 2020 have already churned.  
New customers are more likely to leave than old customers.  
 
<p align="center">
  <img src="/images/seasonality.png" 
  width="600"
  height="400"
  alt="March seasonality">
</p>

The distribution of monthly charges has three peaks at $20, $50, and $80 per month.  
Total charges is highly right skewed, with most people paying close to $0 total and only a few people paying over $6000 over the life of their plan. 
Contract length is bi-modal, with many people having contracts less than 100 months or more than 2000 months.  

#### Train Results

<p align="center">
  <img src="/images/lr_train_results.png"
  width="425"
  height="500"
  alt="Linear regression train results">
</p>

The best model was the LightGBM trained on SMOTE upsampled data.
This model achieved the highest scores on roc-auc and accuracy (ROC-AUC = 0.88, accuracy = 0.81).
The LightGBM Model will be tested on the test set.

<p align="center">
  <img src="/images/rf_train_results.png"
  width="300"
  height="100"
  alt="Random Forest train results">
</p>

The LighGBM Classifier, fit on SMOTE upsampled training data, achieved a lower ROC-AUC on the test set (ROC-AUC = 0.80).
This model is likely slightly overfit but still achieves a reasonabl training score.

#### Test Results

<p align="center">
  <img src="/images/test_results.png"
  width="300"
  height="100"
  alt="Test results">
</p>

### 6. Conclusions and Business Recommendations<a id='conclusions'></a>

#### Conclusions

LightGBM Classifier achieved the best model fit (ROC-AUC<sub>TEST</sub> = 0.80, accuracy<sub>TEST</sub> = 0.80). If this model predicts a customer will churn, there's about 80% chance the customer will actually churn (precision<sub>TEST</sub> = 0.80). Of customers who do churn, the model can be expected to predict about 57% of them (recall<sub>TEST</sub> = 0.57).

#### Business Recommendations 

Telecom can feel confident implementing this model to predict which customers will churn. They can expect that if the model says a customer will churn, it's very likely that that customer will indeed churn. The model may miss some customers who will churn, so it's not a bad idea to offer some additional small promotions across the clientelle. Telecom would do well to focus on keeping new customers, as old customers are likely to continue with the company.  

#### Future Research 

More research should be done on the company's newer customers to determine why they are leaving the company. Follow up surveys and additional promotions with this group could help strengthen new client retention.

