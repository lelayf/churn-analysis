# Maca data scientist screener 

My analysis is done in 3 phases.

## Data preparation

See [0-DataPrep.ipynb](./0-DataPrep.ipynb)

For the data prep I focused on identifying data type inconsistencies, missing values, and getting a feel for the distributional differences of each variables between the training and testing dataset. Almost all discrepancies were found in the testing dataset.

I persisted the resulting cleaned up data in the following Parquet files:
- [training2.pq](./data/training2.pq)
- [testing2.pq](./data/testing2.pq)

## Data exploration

See [1-ExploratoryDataAnalysis.ipynb](./1-ExploratoryDataAnalysis.ipynb)

For this part my focus was to identify distributional associations between the `Churn` target variable and other variables. I focused almost exclusively on the training set, and derived supplemental ordinal variables from the specific thresholds that I was able to visually identify. 

This proved helpful to develop a first idea of potential drivers of Churn:
- Age
- Gender
- Support Calls
- Payment Delays
- Total Spend

I persisted the resulting data in the following Parquet files:
- [training3.pq](./data/training3.pq)
- [testing3.pq](./data/testing3.pq)

## Modelling and retention campaign

See [2-PredictiveModel-NoBinning.ipynb](./2-PredictiveModel-NoBinning.ipynb)

Following the preceding exploration, I was interested in producing an actual predictive model of Churn. The ambition with that effort was:
- to use a type of model that can be easily interpreted
- to use a model that is somewhat insensitive to outliers and does not require scaling features
- to have the ability to control the complexity of the model (in our case, lower is better)
- to further identify important features, and how they rank
- to suggest retention actions based on those interpretations.

For that reason, I decided to use random forests and decision trees. I also tried to use logistic regression, Gradient boosted histograms, and a one-class Support Vector Machine, but none of them performed as well as the decision trees I trained (undocumented for the sake of brevity - please let me know if you want to see the notebook).

One interesting outcome is that the feature that we had identified in phase 2 carried over as important features of our models.

To be noted:
- I used the `rfpimp` package to provide measures of feature importance based on data permutations, as a potentially [more robust approach](https://explained.ai/rf-importance/index.html) that the default mean decrease in impurity (aka gini importance), used by scikit-learn.
- I tried to use the categorical variables I had created in step 2 of this assignment but eventually realized they did not add much value (undocumented for the sake of brevity, again, please let me know if you want to see the notebook).
- I used a random forest to train what is really a single decision tree, multiple times (for different random seeds). Most likely not a problem, but with more time I would use the actual [`DecisionTree`](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html#sklearn.tree.DecisionTreeClassifier) class instead.














