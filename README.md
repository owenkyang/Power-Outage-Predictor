# Analyzing Power Outages 🔌
Project for DSC80 UCSD
by Owen Yang

## Introduction
The United States Department of Energy reports that the U.S economy loses $150 Billion annually from power outages [(Source)](https://www.energy.gov/ne/articles/department-energy-report-explores-us-advanced-small-modular-reactors-boost-grid#:~:text=of%20Nuclear%20Energy-,Department%20of%20Energy%20Report%20Explores%20U.S.%20Advanced,Reactors%20to%20Boost%20Grid%20Resiliency&text=The%20U.S.%20Department%20of%20Energy,reinvested%20back%20into%20the%20economy.). That is a large amount of resources that can be reinvested in back into the economy. Thus, I will be doing an analysis on a data set consisting of all major power outages in the United States from January 2000 to July 2016 acquired from Purdue University's Laboratory for Advancing Substainable Critical Infrastructure [(Dataset)](https://engineering.purdue.edu/LASCI/research-data/outages).

Energy and power have become an essential resource within society. In this era of technology, the AI boom, big tech companies like Meta, Google, and OpenAI use a lot of energy resources to advance societal progressing innovations like training new AI models. However, this leads to a major security risk for both companies and the rest residents of the United States. With these companies being so reliant on energy as resource, they could become a big target to malicious attacks that intend to hinder progress and disrupt American society. This makes it essential to understand why major power outages happen and thus, my analysis will be centered around the question of is there a noticeable trend between the industrial and commercial energy consumption levels and outages caused by malicious attacks?

According to the Department of Energy, a major outage is defined as a power outage that has impacted over 50,000 people or caused an unplanned firm load loss of at least 300MW [Firm Load Loss]. The dataset I will be working with provides information on such outages such as geographical location, date and time of the outages, climate information, land-use characteristics, and the energy consumption, and the economic data of impacted regions. Additionally, the dataset has a total of 1534 rows which describes 1534 outages. This dataset is vast so I will only focus on a few of the columns that are the most relevant to my analysis. Here is a description of the columns I will be utilizing:



## Data Cleaning and Exploratory Data Analysis

### Data Cleaning and Preprocessing
 1. **Dropping the "Units" Row:**
   The first row after skipping the metadata contained units instead of data, so we dropped it.

2. **Mean Imputation for Numerical Columns:**
   For numerical columns, we replaced missing values with the mean value of each column. This included columns such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, `ANOMALY.LEVEL`, `POPULATION`, and `DEMAND.LOSS.MW`.


3. **Mode Imputation for Categorical Columns:**
   For categorical columns, we replaced missing values with the most frequent value (mode) of each column. This included columns such as `CLIMATE.REGION`, `CAUSE.CATEGORY`, `NERC.REGION`, `U.S._STATE`, and `MONTH`.

4. **Combining Date and Time Columns:**
   We combined `OUTAGE.START.DATE` and `OUTAGE.START.TIME` to create a single `OUTAGE_START_DATETIME` column, and similarly, combined `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` to create a single `OUTAGE_RESTORATION_DATETIME` column. This helped reduce redundancy and simplified the dataset.

5. **Dropping Old Date and Time Columns:**
   After creating the datetime columns, we dropped the original date and time columns to avoid redundancy.


### Data Description
The original raw dataset contains 1534 rows, corresponding to 1534 outages, and 57 columns. For the purpose of our analysis, we focused on the following columns:

| Columns | Description |
| ------- | ----------- |
| `OUTAGE_START_DATETIME` | Combined datetime of outage start |
| `OUTAGE_RESTORATION_DATETIME` | Combined datetime of outage restoration |
| `NERC.REGION` | North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outage event |
| `OUTAGE.DURATION` | Duration of outage events (in minutes) |
| `CLIMATE.REGION` | U.S. Climate regions as specified by National Centers for Environmental Information |
| `CAUSE.CATEGORY` | Categories of all the events causing the major power outages |
| `ANOMALY.LEVEL` | Gravity of natural disaster on power outage |
| `U.S._STATE` | State in which the outage occurred |
| `POPULATION` | Population of the affected area |
| `DEMAND.LOSS.MW` | Demand loss in megawatts |
| `MONTH` | Month in which the outage occurred |
| `YEAR` | Year in which the outage occurred |

### Cleaned Data Example
Here is an example of the cleaned dataset:

| OUTAGE_START_DATETIME  | OUTAGE_RESTORATION_DATETIME | NERC.REGION | CUSTOMERS.AFFECTED | OUTAGE.DURATION | CLIMATE.REGION     | CAUSE.CATEGORY    | ANOMALY.LEVEL | U.S._STATE | POPULATION | DEMAND.LOSS.MW | MONTH | YEAR  |
|------------------------|-----------------------------|-------------|--------------------|-----------------|--------------------|-------------------|---------------|------------|------------|----------------|-------|-------|
| 2011-07-01 17:00:00    | 2011-07-03 20:00:00         | MRO         | 70000.000000       | 3060.0          | East North Central | severe weather    | -0.3          | Minnesota  | 5348119.0  | 536.287093     | 7.0   | 2011  |
| 2014-05-11 18:38:00    | 2014-05-11 18:39:00         | MRO         | 143456.222731      | 1.0             | East North Central | intentional attack | -0.1          | Minnesota  | 5457125.0  | 536.287093     | 5.0   | 2014  |
| 2010-10-26 20:00:00    | 2010-10-28 22:00:00         | MRO         | 70000.000000       | 3000.0          | East North Central | severe weather    | -1.5          | Minnesota  | 5310903.0  | 536.287093     | 10.0  | 2010  |
| 2012-06-19 04:30:00    | 2012-06-20 23:00:00         | MRO         | 68200.000000       | 2550.0          | East North Central | severe weather    | -0.1          | Minnesota  | 5380443.0  | 536.287093     | 6.0   | 2012  |
| 2015-07-18 02:00:00    | 2015-07-19 07:00:00         | MRO         | 250000.000000      | 1740.0          | East North Central | severe weather    | 1.2           | Minnesota  | 5489594.0  | 250.000000     | 7.0   | 2015  |

### Exploratory Data Analysis (EDA)
We conducted EDA to understand the distribution and characteristics of the outages, visualizing the data to identify key trends and patterns.

#### Univariate Analysis

<iframe src="assets/climate_region_distribution.html" width="800" height="600" frameborder="0"></iframe>

The graph above represents the distribution of power outages across different climate regions in the United States. This bar plot provides a visual summary of the number of outages recorded in each climate region.

**X-Axis (Index):**
The x-axis represents the different climate regions. Each label on the x-axis corresponds to a specific climate region as defined by the National Centers for Environmental Information. The climate regions included in this dataset are:

- Northeast
- South
- West
- Central
- Southeast
- East North Central
- Northwest
- Southwest
- West North Central

**Y-Axis (Value):**
The y-axis represents the number of power outages. This value indicates how many power outage events were recorded in each respective climate region.

**Observations:**
- The Northeast region has the highest number of recorded power outages, with a count nearing 350 events.
- The South, West, and Central regions also have a relatively high number of outages, each with counts exceeding 200 events.
- The Southeast, East North Central, and Northwest regions have moderate numbers of outages, ranging from approximately 100 to 150 events.
- The Southwest and West North Central regions have the fewest recorded outages, with the West North Central region having significantly fewer events compared to other regions.

This distribution highlights that some regions experience a higher frequency of power outages compared to others. Understanding these regional differences is crucial for targeted improvements in power grid resilience and outage management. The graph helps identify regions that may require more robust infrastructure or more efficient outage response strategies.


#### Bivariate Analysis
**Observations:**
- The plot below shows a wide dispersion of points, indicating significant variability in both outage duration and the number of customers affected.
- There are several clusters of points, especially around shorter outage durations (less than 1000 minutes) and varying numbers of affected customers.
- Some points represent extreme values, with outages lasting up to 7000 minutes and affecting up to 300,000 customers.

<iframe src="assets/outage_duration_vs_customers_affected.html" width="800" height="600" frameborder="0"></iframe>

#### Interesting Aggregates
The pivot table displayed below shows the impact of power outages in different U.S. climate regions. It summarizes the number of outages, the average length of outage duration, and the average number of customers affected per outage. This analysis provides insights into how different climate regions are affected by power outages and the typical duration of these outages.

| CLIMATE.REGION     | Amount of Outages | Average Length of Outage Duration (minutes) | Average Number of Customers Affected |
|--------------------|-------------------|---------------------------------------------|---------------------------------------|
| Central            | 166               | 1424.87                                     | [Insert Value]                        |
| East North Central | 110               | 2666.07                                     | [Insert Value]                        |
| Northeast          | 296               | 1394.34                                     | [Insert Value]                        |
| Northwest          | 125               | 1004.59                                     | [Insert Value]                        |
| South              | 190               | 1211.80                                     | [Insert Value]                        |

- **Central**: The Central region experienced 166 outages with an average duration of approximately 1424.87 minutes. The average number of customers affected per outage in this region can provide further insights.
- **East North Central**: This region had 110 outages with a significantly higher average duration of 2666.07 minutes, indicating potentially more severe outages or slower restoration times.
- **Northeast**: With 296 outages, the Northeast region saw an average outage duration of 1394.34 minutes, which is relatively moderate compared to other regions.
- **Northwest**: The Northwest experienced 125 outages with the shortest average duration of 1004.59 minutes, suggesting more efficient outage management.
- **South**: The South region had 190 outages with an average duration of 1211.80 minutes.

The pivot table reveals that different climate regions experience varying frequencies and durations of power outages. These differences could be attributed to regional climatic conditions, infrastructure robustness, and efficiency of the power restoration processes.

By analyzing this data, utility companies and policymakers can identify regions that may require improved infrastructure or more efficient restoration protocols to reduce the impact of power outages on customers.

## Assessment of Missingness

### NMAR Analysis
To determine whether data are likely NMAR (Not Missing At Random), we must reason about the data-generating process. In our dataset, if the missingness of the `OUTAGE.DURATION` is due to factors not recorded in the dataset, such as manual errors during data entry or specific reporting practices of certain regions, it could be NMAR. However, we cannot conclude this solely by looking at the data. Additional information about the data collection process would be necessary to determine NMAR.

### Missingness Dependency
We analyzed the dependency of the missingness of the `OUTAGE.DURATION` column on other columns in the dataset by performing permutation tests.

### Missingness Summary

| Column                      | Missingness Proportion |
|-----------------------------|------------------------|
| `OUTAGE_START_DATETIME`       | 0.005867               |
| `OUTAGE_RESTORATION_DATETIME` | 0.037810               |
| `NERC.REGION`                 | 0.000000               |
| `CUSTOMERS.AFFECTED`          | 0.288787               |
| `OUTAGE.DURATION`             | 0.037810               |
| `CLIMATE.REGION`              | 0.003911               |
| `CAUSE.CATEGORY`              | 0.000000               |
| `ANOMALY.LEVEL`               | 0.005867               |
| `MONTH`                       | 0.005867               |
| `OUTAGE_DURATION_MISSING`     | 0.000000               |

### Permutation Test Results
#### Categorical Column: CAUSE.CATEGORY
- Observed TVD Statistic: 0.2520091580226147
- P-value: 0.0

#### Categorical Column: CLIMATE.REGION
- Observed TVD Statistic: 0.2535212947392274
- P-value: 0.004

#### Categorical Column: NERC.REGION
- Observed TVD Statistic: 0.3153910849453322
- P-value: 0.0

#### Categorical Column: ANOMALY.LEVEL
- Observed TVD Statistic: 0.4918007853547923
- P-value: 0.0

#### Categorical Column: MONTH
- Observed TVD Statistic: 0.22267850229522704
- P-value: 0.118

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
