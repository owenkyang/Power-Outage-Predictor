# Power Outage Cause Predictor 🔌
Project for DSC80 UCSD
by Owen Yang

## Introduction
The United States Department of Energy reports that the U.S economy loses $150 Billion annually from power outages [(Source)](https://www.energy.gov/ne/articles/department-energy-report-explores-us-advanced-small-modular-reactors-boost-grid#:~:text=of%20Nuclear%20Energy-,Department%20of%20Energy%20Report%20Explores%20U.S.%20Advanced,Reactors%20to%20Boost%20Grid%20Resiliency&text=The%20U.S.%20Department%20of%20Energy,reinvested%20back%20into%20the%20economy.). That is a large amount of resources that can be reinvested in back into the economy. Thus, I will be doing an analysis on a data set consisting of all major power outages in the United States from January 2000 to July 2016 acquired from Purdue University's Laboratory for Advancing Substainable Critical Infrastructure [(Dataset)](https://engineering.purdue.edu/LASCI/research-data/outages).

Energy and power have become an essential resource within society. In this era of technology, the AI boom, big tech companies like Meta, Google, and OpenAI use a lot of energy resources to advance societal progressing innovations like training new AI models. However, this leads to a major security risk for both companies and the rest residents of the United States. With these companies being so reliant on energy as resource, they could become a big target to malicious attacks that intend to hinder progress and disrupt American society. This makes it essential to understand why major power outages happen and thus, my analysis will be centered around the question of **is there a noticeable trend between the industrial and commercial energy consumption levels and outages caused by malicious attacks?**

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
   my data less wide. Additionally, I renamed U.S._STATE to US.STATE for simplicity.


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
There is a ton of missingness within this dataset. However, one of these columns is NMAR (Not missing at random) and that column is DEMAND.LOSS.MW. To determine if a column is NMAR, we have to analyze the data generating process. We can justify that the DEMAND.LOSS.MW column is NMAR since during the data collection process of Demand Loss, it is a metric that is easy for companies to forget to report or there was errors in the collecting process.

We can determine if DEMAND.LOSS.MW was MAR if we had access to other data such as what companies self-reported the demand loss and see if they have a history of not reporting demand loss.

### Missingness Dependency
I analyzed the missingess dependency of the INDUSTRY.CONSUMPTION on other columns. The dependency will be tested on the columns of `NERC.REGION` and `MONTH` through permutation tests.

### Missing Dependency on NERC.REGION

**Null Hypothesis:** The Distribution of the Nerc Region Category is the same when Industry Consumption is missing vs. when Industry Consumption is not missing

**Alternative Hypothesis:** The Distribution of Nerc Region Category is not the same when Industry Consumption is missing vs. when Industry Consumption is not missing

**Test Stat:** TVD

**Significance Level** = 0.05

<iframe src="assets/miss1.html" width="500" height="350" frameborder="0"></iframe>

From the observed distribution, I calculated a TVD of 0.246.

<iframe src="assets/missdist1.html" width="500" height="350" frameborder="0"></iframe>

The empirical distribution of TVDs is shown above after running through 1000 iterations. We obtained a p-val of 0.124 and fail to reject the null as a result. This means that our missingness of `INDUSTRY.CONSUMPTION` is not signficiantly dependent on `NERC.REGION`

### Missing Dependency on MONTH

**Null Hypothesis:** The Distribution of the Month Category is the same when Industry Consumption is missing vs. when Industry Consumption is not missing

**Alternative Hypothesis:** The Distribution of Month Category is not the same when Industry Consumption is missing vs. when Industry Consumption is not missing

**Test Stat:** TVD

**Significance Level** = 0.05

<iframe src="assets/miss2.html" width="500" height="350" frameborder="0"></iframe>

From the observed distribution, I calculated a TVD of 0.444.

<iframe src="assets/missdist2.html" width="500" height="350" frameborder="0"></iframe>

The empirical distribution of TVDs is shown above after running through 1000 iterations. We obtained a p-val of 0.0 and reject the null as a result. This means that our missingness of `INDUSTRY.CONSUMPTION` is signficiantly dependent on `MONTH`.

## Hypothesis Testing

Going back to our research question of **is there a noticeable trend between the industrial and commercial energy consumption levels and outages caused by malicious attacks?** I will conduct a permutation test to see if there is a correlation between industrial and commercial energy consumption and outages caused by intentional attacks.

**Null Hypothesis**: On Average, the Industry Energy Consumption Percentage during the time of outages caused by Intentional Attacks are the same as outages caused by Severe Weather.

**Alternative Hypothesis**: On Average, the Industry Energy Consumption Percentage during the time of outages caused by Intentional Attacks are greater than outages caused by Severe Weather.

**Test Stat**: Difference in Means

**Significance Level**: 0.05

The reason I chose Difference in Means for my statistic is I am trying to see if there is a difference in between two distributions. Additionally, there is a direction for my hypothesis where I am trying to determine if the average Industry Consumption percentage is greater during Intentional attacks.

<iframe src="assets/hyp.html" width="800" height="600" frameborder="0"></iframe>

The graph shown above displays the empirical distribution of the difference of means after running the permutation test through 1000 iterations. I calculated an **observed difference of means of 0.873** resulting in a **p-val of 0.011**, and thus, we reject the null.

## Framing a Prediction Problem
I will be implementing a classification model that will be predicting the cause of a power outage. Being able to predict the cause of outages will allow us to become more prepared for future outages and progress towards implementing preventative measures against power outages. Additionally, it allows us to act quicker on power outages reducing downtime and minimizing finanical loss. I will be performing multi-class classification since there are multiple outage cause categories and will be using F1-score to evaluate my model. The reason I am using the F1-score is because there's an unbalanced distribution of outage causes within the provided dataset.

However, to realistically use a model to predict the cause of outages, we can only use features of the data that we would know before an outage such as the U.S. State and Population of the region.  We would not be allowed to use features that correlate to the intensity of the outage as those features are collected after the outage actually happens.

## Baseline Model

### Model Description
First, we will being with implementing the base model. I will be using a Decision Tree Classifier as the model and I will describe the features and feature engineering I will conduct:

### Features and Feature Engineering
The features in the based model included two nominal features, `US.STATE` and `NERC.REGION`:

Since both features are categorical, we encoded them using One-Hot encoding to make the features usable for classification. The reason I chose `US.STATE` and `NERC.REGION` as features is because these two features represent the regions of the outages. Varying on regions like from the west coast where a lot of earthquakes are experienced, these two features make classification of the causes for outages more accurate.

### Data Splitting
The training data was split 75-25 where 25% of the data was set aside for testing.

### Model Evaluation and Assessment
With the utilization of only 2 features and no hyperparameter optimizing (Scikit-learns default parameters for Decision Tree Classifiers were used), the model achieved a F1 score of 0.739. Our Base Model performed poorly due to the fact that we only used two features and conducted no hyperparamter searching. We will see a huge improvement below in the **Final Model**.

## Final Model

Now, we improve on the Base Model that was implemented. I will introduce new features and conduct Hyperparameter Optimization in order to improve classification accuracy.

### Changing the Model

In the Baseline model, we used the Decision Tree Classifier model. However, after experimenting, I switched to a **Random Forest Classifier** in order to reduce the risk of overfitting.

### Added Features and Feature Engineering

`MONTH` (Nominal):

`POPULATION` (Quantitative):

`INDUSTRY.CONSUMPION` (Quantitative): 

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
