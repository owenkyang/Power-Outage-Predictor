# Power Outage Cause Predictor 🔌
Project for DSC80 UCSD
by Owen Yang

## Introduction
The United States Department of Energy reports that the U.S economy loses $150 Billion annually from power outages [(Source)](https://www.energy.gov/ne/articles/department-energy-report-explores-us-advanced-small-modular-reactors-boost-grid#:~:text=of%20Nuclear%20Energy-,Department%20of%20Energy%20Report%20Explores%20U.S.%20Advanced,Reactors%20to%20Boost%20Grid%20Resiliency&text=The%20U.S.%20Department%20of%20Energy,reinvested%20back%20into%20the%20economy.). That is a large amount of resources that can be reinvested in back into the economy. Thus, I will be doing an analysis on a data set consisting of all major power outages in the United States from January 2000 to July 2016 acquired from Purdue University's Laboratory for Advancing Substainable Critical Infrastructure [(Dataset)](https://engineering.purdue.edu/LASCI/research-data/outages).

Energy and power have become an essential resource within society. In this era of technology, the AI boom, big tech companies like Meta, Google, and OpenAI use a lot of energy resources to advance societal progressing innovations like training new AI models. However, this leads to a major security risk for both companies and the rest residents of the United States. With these companies being so reliant on energy as resource, they could become a big target to malicious attacks that intend to hinder progress and disrupt American society. This makes it essential to understand why major power outages happen and thus, my analysis will be centered around the question of is there a noticeable trend between the industrial and commercial energy consumption levels and outages caused by malicious attacks?

According to the Department of Energy, a major outage is defined as a power outage that has impacted over 50,000 people or caused an unplanned firm load loss of at least 300MW [Firm Load Loss]. The dataset I will be working with provides information on such outages such as geographical location, date and time of the outages, climate information, land-use characteristics, and the energy consumption, and the economic data of impacted regions. Additionally, the dataset has a total of 1534 rows which describes 1534 outages. This dataset is vast so I will only focus on a few of the columns that are the most relevant to my analysis. Here is a description of the columns I will be utilizing:

| Columns | Description |
| ------- | ----------- |
| `NERC.REGION` | North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outage event |
| `OUTAGE.DURATION` | Duration of outage events (in minutes) |
| `POPULATION` | Population of the affected area |
| `U.S._STATE` | State in which the outage occurred |
| `CAUSE.CATEGORY` | Categories of all the events causing the major power outages |
| `DEMAND.LOSS.MW` | Demand loss in megawatts |
| `YEAR` | Year in which the outage occurred |
| `MONTH` | Month in which the outage occurred |
| `DEMAND.LOSS.MW` | Demand loss in megawatts |
| `IND.PERCEN` | Percentage of industrial electricity consumption compared to the total electricity consumption in the state (in %) |
| `IND.SALES` | Electricity consumption in the industrial sector (megawatt-hour) |
| `COM.PERCEN` | Percentage of commercial electricity consumption compared to the total electricity consumption in the state (in %) |
| `COM.SALES` | Electricity consumption in the commercial sector (megawatt-hour) |

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning and Preprocessing
 1. **Dropping Unneccesary Columns:**
    I drop every column that I do not need to use for my analysis. This includes every column that is not listed above.
    
 2. **Single Value Imputation Using Means for Quantitative Columns:**
    Next, my Quantitative columns have some missingness to them. To counteract this, I impute these values with the means of such columns. These columns include: `IND.PERCEN`, `IND.SALES`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`,   
    `COM.SALES`, `COM.PERCEN`, and `CUSTOMERS.AFFECTED`.
    
 3. **Single Value Imputation Using Mode for Categorical Columns:**
    Next, my Categorical columns have some missing values as well. To counteract this, I impute the values with the mode of such columns. There's only one categorical column that I need with missingness which is: `MONTH`.

 4. **Combining Columns:**
   Next, I combine the columns of `IND.SALES` and `COM.SALES` together into `INDUSTRY.CONSUMPTION` in order to make my data less wide. This makes it easier to do calculations and analysis with Non-Residential Consumption
   amounts. Additionally, I do the same with the columned `IND.PERCEN` and `COM.PERCEN` into `INDUSTRY.PERCENTAGE`for the same reasons. I then drop the `IND.SALES`, `COM.SALES`, `IND.PERCEN` and `COM.PERCEN` in order to make
   my data less wide.


### Cleaned Data Example
Here is an example of the cleaned dataset:

|   YEAR |   MONTH | US.STATE   |   POPULATION |   OUTAGE.DURATION |   DEMAND.LOSS.MW | CAUSE.CATEGORY     |   CUSTOMERS.AFFECTED | NERC.REGION   |   INDUSTRY.CONSUMPTION |   INDUSTRY.PERCENTAGE |
|-------:|--------:|:-----------|-------------:|------------------:|-----------------:|:-------------------|---------------------:|:--------------|-----------------------:|----------------------:|
|   2011 |       7 | Minnesota  |  5.34812e+06 |              3060 |          536.287 | severe weather     |                70000 | MRO           |            4.22806e+06 |               64.4275 |
|   2014 |       5 | Minnesota  |  5.45712e+06 |                 1 |          536.287 | intentional attack |               143456 | MRO           |            3.69568e+06 |               69.938  |
|   2010 |      10 | Minnesota  |  5.3109e+06  |              3000 |          536.287 | severe weather     |                70000 | MRO           |            3.75298e+06 |               71.867  |
|   2012 |       6 | Minnesota  |  5.38044e+06 |              2550 |          536.287 | severe weather     |                68200 | MRO           |            3.9342e+06  |               67.9827 |
|   2015 |       7 | Minnesota  |  5.48959e+06 |              1740 |          250     | severe weather     |               250000 | MRO           |            3.93955e+06 |               65.9853 |

### Exploratory Data Analysis (EDA)
Next, I conducted EDA on the cleaned data set to discover patterns and seek out interesting distributions.

#### Univariate Analysis

<iframe src="assets/uni1.html" width="800" height="600" frameborder="0"></iframe>
The graph above represents the distribution of industry energy consumption percentage or non-residential energy consumption percentage. The x-axis displays the percentage bins with the y-axis displays the frequency of those percentages. 

**Observations:**
The distribution is almost normal, with most consumption percentages between 60% and 65%. This shows that the industrial and commercial sectors used more energy than the residential sector in most regions.

<iframe src="assets/uni2.html" width="800" height="600" frameborder="0"></iframe>
The above graph displays the distribution of outages per cause category. The x-axis displays the different cause categories and the y-axis displays the amount of outages that are caused by each category.

**Observations:**
We can see that severe weather followed by intentional attacks cause most outages, the cause we're most interested in. We then see that the outages caused by the other factors are quite rare compared to severe weather and intentional attacks.

<iframe src="assets/map2.html" width="800" height="600" frameborder="0"></iframe>
I also wanted to see the distribution of the number of outages per state. The graph above displays how many outages happened in each U.S. State.

**Observations:**
We can see that most outages happen in California and Texas!

#### Bivariate Analysis

<iframe src="assets/bi1.html" width="800" height="600" frameborder="0"></iframe>
The graph above displays the relationship between Industry Energy Consumption and Years.

**Observations:**
There seems to be no coherent trend between years and industry energy consumption!

<iframe src="assets/bi3.html" width="800" height="600" frameborder="0"></iframe>
The graph above displays the Industry Energy Consumption percentage for each outage cause.

**Observations:**
We can see that both outages caused by severe weather and intentional attacks both typically have a higher industrial energy consumption percentage!

#### Interesting Aggregates
| CAUSE.CATEGORY                |   INDUSTRY.CONSUMPTION |   INDUSTRY.PERCENTAGE |   OUTAGE.DURATION |
|:------------------------------|-----------------------:|----------------------:|------------------:|
| equipment failure             |            9.6593e+06  |               63.3537 |          1884.28  |
| fuel supply emergency         |            1.02709e+07 |               64.296  |         10716.1   |
| intentional attack            |            4.41917e+06 |               62.9796 |           508.763 |
| islanding                     |            1.00101e+07 |               66.0062 |           305.974 |
| public appeal                 |            9.94388e+06 |               59.8695 |          1468.45  |
| severe weather                |            7.41852e+06 |               62.1058 |          3852.64  |
| system operability disruption |            1.04029e+07 |               62.8009 |           788.603 |

The aggregated dataframe above displays the average Industry Consumption, the average Industry Consumption Percentage, and the Outage Duration for each outage cause category.

We can see that equipment failure and public appeal have the highest average amount of industry consumption. Additionally we can see that outages caused by islanding have the highest industry consumption percentage on average, and that outages caused by fuel supply emergancy last the longest on average.

The aggregated table reveals that outages caused by different factors can result in different outage durations. The table also reveals that there might be an underlying relationship between Industry Energy Consumption levels and the cause of an outage. By looking at this data, companies and society can determine which outages last the longest and see if there is an underlying relationship by how we consume our resources.

## Assessment of Missingness

### NMAR Analysis
To determine whether data are likely NMAR (Not Missing At Random), we must reason about the data-generating process. In our dataset, if the missingness of the `OUTAGE.DURATION` is due to factors not recorded in the dataset, such as manual errors during data entry or specific reporting practices of certain regions, it could be NMAR. However, we cannot conclude this solely by looking at the data. Additional information about the data collection process would be necessary to determine NMAR.

### Missingness Dependency
We analyzed the dependency of the missingness of the `OUTAGE.DURATION` column on other columns in the dataset by performing permutation tests.


### Interpretation
The p-values obtained from the permutation tests indicate that the missingness of the `OUTAGE.DURATION` column is dependent on `CAUSE.CATEGORY`, `CLIMATE.REGION`, `NERC.REGION`, and `ANOMALY.LEVEL` (p-value < 0.05). However, the missingness of `OUTAGE.DURATION` is not dependent on `MONTH` (p-value = 0.118).

### Detailed Analysis
#### MONTH
The p-value of 0.118 for the `MONTH` column suggests that the missingness of `OUTAGE.DURATION` is not significantly dependent on the month in which the outage occurred. This implies that the occurrence of missing data for outage duration does not vary significantly across different months. The permutation test's observed statistic for `MONTH` fell within the distribution of the permuted statistics, indicating no strong relationship between `MONTH` and missingness in `OUTAGE.DURATION`.

#### CLIMATE.REGION
On the other hand, the `CLIMATE.REGION` column shows a significant dependency on the missingness of `OUTAGE.DURATION` with a p-value of 0.004. This indicates that the missing data for outage duration is not uniformly distributed across different climate regions. The observed statistic for `CLIMATE.REGION` is significantly higher than most of the permuted statistics, suggesting that certain climate regions may have more or less missing data, possibly due to varying reporting standards or environmental factors affecting data collection.

### Graphs
#### Permutation Test: MONTH and Missingness of OUTAGE_DURATION
The following plot shows the distribution of test statistics from the permutation test for the `MONTH` column. The red dashed line represents the observed test statistic.

<iframe src="assets/permutation_test_MONTH.html" width="800" height="600" frameborder="0"></iframe>

#### Permutation Test: CAUSE.CATEGORY and Missingness of OUTAGE_DURATION
The following plot shows the distribution of test statistics from the permutation test for the `CAUSE.CATEGORY` column. The red dashed line represents the observed test statistic.

<iframe src="assets/permutation_test_CAUSE_CATEGORY.html" width="800" height="600" frameborder="0"></iframe>

## Hypothesis Testing
We formulated and tested hypotheses to explore the relationship between climate regions and the duration of power outages. Specifically, we examined if the climate region has an effect on the duration of power outages.

### Null Hypothesis (H₀)
The climate region has no effect on the duration of power outages.

### Alternative Hypothesis (H₁)
The climate region does have an effect on the duration of power outages.

### Permutation Test
To test these hypotheses, we performed a permutation test with the following steps:

1. **Calculate the Observed Statistic:**
   - We calculated the difference in means of `OUTAGE.DURATION` across different `CLIMATE.REGION` values.

2. **Generate Permutation Samples:**
   - We randomly shuffled the `CLIMATE.REGION` labels and recalculated the difference in means to generate a distribution of the test statistic under the null hypothesis.

3. **Calculate P-value:**
   - We computed the p-value by comparing the observed test statistic to the distribution of permuted test statistics.

### Results
The observed statistic and p-value obtained from the permutation test are as follows:

- **Observed Statistic:** 931.7692073170731
- **P-value:** 0.453

#### Interpretation
Since the p-value is greater than 0.05, we fail to reject the Null Hypothesis. This suggests that there is no significant effect of climate regions on the duration of power outages.

### Graphical Representation
The following plot shows the distribution of the test statistics from the permutation test for the `CLIMATE.REGION` column. The red dashed line represents the observed test statistic.

<iframe src="assets/hypothesis_permutation.html" width="800" height="600" frameborder="0"></iframe>

This graphical representation further confirms our statistical findings. The observed test statistic falls well within the distribution of permuted statistics, indicating that the observed difference is not significantly different from what we would expect by random chance.

Thus, we conclude that there is no significant evidence to suggest that climate regions have an effect on the duration of power outages.

## Framing a Prediction Problem
We framed a prediction problem to identify the most important causes and characteristics of major power outages. The goal was to build a model that can predict the duration of an outage based on given conditions, which can help energy companies implement preventative measures. For this prediction problem, we will use regression.

### Prediction Task
The specific prediction task is to predict the duration of a power outage based on the climate region, taking into account other relevant features.

### Response Variable
Our response variable is `OUTAGE.DURATION`, as this is the column we are trying to predict.

### Evaluation Metric
The metric we plan on using is Root Mean Squared Error (RMSE). RMSE is chosen because it penalizes large errors more due to the squaring of the residuals. This makes it a good choice for our problem where we want to minimize the difference between the actual and predicted duration of a power outage based on the climate region.

### Features
The features we will use in our model are:
- `CLIMATE.REGION`: The U.S. Climate regions as specified by the National Centers for Environmental Information.
- `CUSTOMERS.AFFECTED`: Number of customers affected by the power outage event.
- `ANOMALY.LEVEL`: Gravity of natural disaster on power outage.
- `NERC.REGION`: North American Electric Reliability Corporation regions involved in the outage event.
- `POPULATION`: Population in the U.S. state in a year.
- `CAUSE.CATEGORY`: Categories of all the events causing the major power outages

## Baseline Model

### Model Description
In this step, we developed a baseline model to predict the duration of power outages. A baseline model serves as a starting point for comparing more complex models in subsequent steps. For our baseline model, we selected two features: `CLIMATE.REGION` and `CAUSE.CATEGORY`. These features were chosen based on their relevance to the central question of our project, which explores how climate regions affect the duration of power outages.

### Features and Encoding
The features used in our baseline model include both nominal variables. To incorporate these features into our model, we performed the following preprocessing steps:

- **CLIMATE.REGION** (Nominal): This feature represents the climate region where the outage occurred. Since `CLIMATE.REGION` is a categorical variable, we utilized one-hot encoding to convert it into a format suitable for the linear regression model. One-hot encoding transforms categorical variables into a series of binary columns, each representing a unique category within the original feature. This allows the model to interpret the categorical data appropriately.

- **CAUSE.CATEGORY** (Nominal): This feature represents the cause of the outage. Similar to `CLIMATE.REGION`, we applied one-hot encoding to `CAUSE.CATEGORY` to handle its categorical nature. By converting the cause categories into binary columns, the model can effectively process and utilize this information for prediction.

### Data Splitting
To ensure that our model's performance is evaluated on unseen data, we split the dataset into training and test sets. We used a 70-30 split ratio, where 70% of the data was used for training the model, and 30% was reserved for testing. This split helps us assess the model's ability to generalize to new data and prevents overfitting to the training set.

### Model Pipeline
We constructed a pipeline using `scikit-learn` to streamline the modeling process. The pipeline consists of preprocessing steps for encoding the categorical features and a linear regression model for prediction. The pipeline ensures that the data undergoes consistent preprocessing and modeling steps, making the process more efficient and reproducible.

The pipeline includes the following steps:
1. **Preprocessor**: This step handles the one-hot encoding of the categorical features `CLIMATE.REGION` and `CAUSE.CATEGORY`.
2. **Regressor**: This step applies a linear regression model to the preprocessed data to predict the duration of power outages.

### Model Training and Evaluation
The pipeline was trained on the training set, and the model's performance was evaluated using the test set. The primary evaluation metric used was Root Mean Squared Error (RMSE). RMSE is a widely used metric that measures the average magnitude of the errors between predicted and actual values. It penalizes larger errors more heavily due to the squaring of residuals, making it a suitable choice for our prediction task.

### Performance Evaluation
The RMSE obtained for the baseline model on the test set is `1363.7571213626188`. This value serves as a benchmark for comparing more complex models that we will develop in subsequent steps. The baseline model's performance provides a reference point against which we can measure improvements achieved through feature engineering and hyperparameter tuning.

### Model Assessment
Based on the RMSE value, the baseline model provides a foundational understanding of the factors affecting outage duration. While the current RMSE is relatively high, it is expected for a baseline model. This performance indicates that there is significant room for improvement, which we aim to achieve by incorporating additional features, performing hyperparameter tuning, and exploring more sophisticated modeling techniques in the subsequent steps.

The baseline model is considered "good" in the context of establishing a reference point for future models. It offers initial insights into the relationship between climate regions, the causes of outages, and the duration of outages. However, further refinement and feature engineering are necessary to enhance the model's predictive accuracy and reduce the RMSE. By building on this baseline, we can develop more robust models that provide deeper insights and more accurate predictions.


## Final Model

In this section, we create a "final" model that improves upon the "baseline" model created in the Baseline Model step. We do so by engineering at least two new features from the data, on top of any categorical encodings performed in the Baseline Model Step.

### New Features

We added the following features:

- `CUSTOMERS.AFFECTED`: This feature directly measures the number of customers affected by each outage event. The rationale for this is as the amount of customers affected by a power outage increases, the duration decreases due to the severity of the outage
- `NERC.REGION`: The rational for adding this feature is that different NERC regions may have varying infrastructure quality and resilience. Some regions might have more robust systems that can restore power more quickly.
- `ANOMALY.LEVEL`: Anomaly levels often indicate extreme weather or environmental conditions (e.g., unusually high temperatures, severe storms, or other unusual weather events) which can significantly impact the power grid's stability and lead to longer outages.
- `POPULATION`: The total population in the affected area can be a crucial factor in predicting the duration of an outage. Areas with higher populations might have more robust infrastructure to handle outages, meaning they have lower outages durations.
- `CAUSE.CATEGORY`: Different causes of outages (e.g., weather-related, equipment failure, human error) have distinct characteristics and challenges. Understanding the cause helps in estimating how long it might take to address the issue.

### Model Selection and Hyperparameter Tuning

To select the best model, we considered several different modeling algorithms and performed hyperparameter tuning using `GridSearchCV`. The algorithms considered include:

- **Linear Regression**
- **Random Forest Regressor**
- **Lasso**
- **Support Vector Regressor (SVR)**

We decided to use `RandomForestRegressor` as our final model based on its performance during the tuning process. Below, we describe the hyperparameters tuned and the rationale behind their selection:

- **Number of Estimators**: Number of trees in the forest. Tested values were [100, 200].
- **Maximum Depth**: Maximum depth of the trees. Tested values were [10, 20].
- **min_samples_split**: Minimum number of samples required to split an internal node. Tested values were [2, 5].

The best performing hyperparameters that resulted in the lowest Root Mean Squared Error (RMSE) achieved during the cross-validation process in GridSearchCV were:
- n_estimators: 200
- max_depth: 10
- min_samples_split: 10

### Training and Evaluation

To identify the most effective features, we trained our model five times using various combinations of features. We then created a dictionary, where the keys represent the RMSE values and the corresponding feature combinations are the values. Finally, we sorted the dictionary in ascending order based on RMSE, ensuring the smallest RMSE appears first.

We trained our model using the same unseen and seen datasets from the baseline model. This ensured that the evaluation metric obtained on the final model could be compared to the baseline model's on the basis of the model itself and not the dataset it was trained on.

The RMSE values for all of the different models are shown in the plot below:

As you can see in the graph, the model with the lowest RSME has the features `CUSTOMERS.AFFECTED`, `CLIMATE.REGION`, `NERC.REGION`, `ANOMALY.LEVEL`, `CAUSE.CATEGORY`

![RMSE PLOT](assets/Screenshot 2024-06-12 at 7.22.29 PM.png)

### Conclusion

The final model showed an improvement over the baseline model. The engineered features and tuned hyperparameters contributed to better model performance. The `RandomForestRegressor` was chosen as the final model due to its ability to handle complex interactions between features and its robustness to overfitting when properly tuned.

The plot above illustrates the RMSE values for different models, highlighting the improvement achieved with our final model.

By carefully selecting features and tuning hyperparameters, we were able to create a model that performs better than the baseline, thus demonstrating the importance of thoughtful feature engineering and model selection in predictive modeling.

### Fairness Analysis

## Evaluation Metric
- The evaluation metric used is the Root Mean Squared Error (RMSE), a standard measure for assessing the accuracy of a regression model. It quantifies the difference between the predicted values and the actual values.

## Group Definition
- **Group X** = not_intentional (contains severe weather, system operability disruption, equipment failure and fuel supply emergency)
- **Group Y** = Everything else (intentional attack, public appeal, islanding)

## Hypotheses
- **Null Hypothesis (H0)**: The model is fair across the different CAUSE.CATEGORY. The RMSE for Group X and Group Y are approximatley the same, and any observed differences are due to random chance.
- **Alternative Hypothesis (H1)**: The model is unfair, showing a significant difference in RMSE between Group X (not_intentional) and Group Y (Everything Else).

## Test Statistic and Significance Level
- The test statistic is the absolute difference in RMSE between the two groups. The permutation test involves shuffling the ‘CAUSE.CATEGORY’ labels and recalculating this difference. The significance level we choose is 0.05 as it is a common metric of significance.

## P-Value and Conclusion
- After performing the permutation test for 1,000 iterations, the p-value is determined by calculating the proportion of permutations in which the observed test statistic is at least as extreme as the test statistic from the original dataset.

- **P-Value**: 0.15

- **Conclusion**: With a p-value of 0.15, which is above the commonly used significance level of 0.05, we do not have sufficient evidence to reject the null hypothesis. This means that we cannot conclude that there is a significant difference in RMSE between the two groups, and the observed difference is likely due to random chance.



## Report Conclusion
Our analysis provides valuable insights into the causes and characteristics of major power outages in the United States. The cleaned and preprocessed dataset, along with our exploratory data analysis and hypothesis testing, helped us understand the key factors affecting power outages. Our predictive model can assist energy companies in implementing preventative measures to minimize the impact of power outages on customers.

By conducting a fairness analysis, we ensured that our model is equitable and does not disproportionately affect certain groups. This comprehensive approach allows us to make informed recommendations for improving the resilience of the power grid and enhancing the reliability of electricity supply.

Overall, our project demonstrates the importance of data-driven decision-making in addressing complex challenges in the energy sector.
