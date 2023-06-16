# Data Prediction on the food receipes and ratings reviews since 2008

by Su Aye 
---
### Prediction: Binary Rating of the ratings

## Introduction

In order to predict the binary rating of a given item, my objective is to develop a model that can accurately classify the ratings into two categories: 0 and 1. For this task, we will consider ratings ranging from 0 to 3 as category 0, and ratings of 4 and 5 as category 1. By building a predictive model, I aim to leverage various features and patterns present in the dataset to make accurate predictions about the binary rating. The model will be trained on historical data where the binary rating has already been assigned, enabling it to learn from past patterns and generalize its predictions to new, unseen data. My ultimate goal is to create a robust model that can reliably classify future ratings into the appropriate binary categories, providing valuable insights and aiding decision-making processes.

Exploratory data analysis on this dataset can be found [here.](https://suaye07.github.io/Analysis-of-recipes-and-ratings/)

---

## Framing the Problem 

***Data Cleaning***

To create the model, I first prepared the data by cleaning it according to the requirements. I reused the dataset that was already cleaned in a previous project 3. Before starting to build the model, let's take a look at how the data table appears in its original form. This will give us an idea of what the data looks like before we begin working on the model. 


|    |   minutes | tags                                                                                                                                                                                                                                                                                                                                                                                                    |   n_steps |   n_ingredients |   binary_rating |
|---:|----------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|----------------:|
|  0 |        40 | ['60-minutes-or-less', 'time-to-make',.....]                                                                                                                                                                                   |         9 |              10 |               1 |
|  2 |        22 | ['30-minutes-or-less', 'time-to-make',.....]                                                                                                                                                                |        14 |              14 |               1 |
|  3 |        22 | ['30-minutes-or-less', 'time-to-make', .....]                                                                                                                                                                |        14 |              14 |               1 |
|  4 |        40 | ['60-minutes-or-less', 'time-to-make',.....]                                                                                                                                                                                                                                                                                                     |         7 |              12 |               1 |


***Problem Identification***

The prediction problem at hand involves determining the rating of a restaurant's overall recipe. This is a classification problem, specifically binary classification. The response variable, or the variable being predicted, is whether the restaurant's overall recipe has received reviews with a mean rating above 4 stars. This variable was chosen as it provides a clear threshold to differentiate between positive and negative evaluations.

To evaluate the model, the chosen metric is accuracy. Accuracy measures the proportion of correctly classified instances, which is suitable for assessing the performance of a binary classification model. By focusing on accuracy, we can gauge the overall effectiveness of the model in correctly predicting whether a restaurant's recipe is above 4 stars or not.

While other suitable metrics such as the F1-score could be considered, accuracy was preferred in this case due to its simplicity and ease of interpretation. Given that the objective is to determine if the overall recipe meets a certain threshold, accuracy provides a straightforward measure of the model's success in achieving this goal.


## Baseline Model 

#### Accuracy: 0.8613529805760214

The model used in this project is a Random Forest Classifier, which is a machine learning algorithm capable of performing classification tasks. The model aims to predict the binary rating of a restaurant's overall recipe, whether it is above 4 stars or not.

The features included in the model are 'n_steps' and 'minutes', representing quantitative variables related to the recipe. These features are used to train the model and make predictions.

To handle the quantitative features, a preprocessing step is performed using the StandardScaler. This scaler standardizes the features by subtracting the mean and scaling to unit variance, ensuring that the variables are on a similar scale and preventing any bias in the model.

The dataset is split into training and testing sets using the train_test_split function, with 80% of the data used for training and 20% for testing.

The preprocessor is applied to the training data using the ColumnTransformer, which specifies that the 'numeric' transformer (StandardScaler) should be applied to the 'n_steps' and 'minutes' columns.
The model is then built using a pipeline that combines the preprocessor and the Random Forest Classifier. The pipeline ensures that the preprocessing steps are applied consistently to the training and testing data.
Once the model is trained, predictions are made on the testing data using the predict function. The accuracy of the model is evaluated by comparing the predicted ratings (y_pred) with the actual ratings (y_test) using the accuracy_score function.

The accuracy of the model is reported to be approximately 86.14%. This means that the model correctly classified 86.14% of the instances in the testing data.

Accuracy is a commonly used metric to evaluate classification models as it provides a straightforward measure of overall correctness. In this case, the accuracy score indicates how well the model performed in predicting whether a restaurant's overall recipe received a rating above 4 stars or not.

A higher accuracy score suggests that the model is effective in making accurate predictions. However, it is important to consider other factors specific to the problem at hand, such as the consequences of false positives or false negatives, the distribution of the classes, and any class imbalance.

While accuracy is a valuable metric, it may not be the sole determinant of the model's "goodness" in every scenario. Additional evaluation and analysis, considering domain knowledge and specific requirements, are necessary to determine the model's suitability and effectiveness in addressing the problem.


## Final Model 

#### Accuracy: 0.930043908610553

Two new features, 'n_ingredients' and 'tags', were added to the dataset. These features have the potential to improve the model's accuracy in predicting the binary rating.

n_ingredients: The number of ingredients required for a recipe can provide insights into the recipe's complexity or simplicity. By including this feature, the model can capture any relationships between the number of ingredients and the rating. For example, recipes with a higher number of ingredients might be seen as more sophisticated, potentially leading to higher ratings. Conversely, recipes with a smaller number of ingredients might be preferred by users seeking simplicity.

tags: The 'tags' feature, representing categorical information about recipe attributes, can offer valuable information for predicting the rating. By including this feature, the model can identify patterns or relationships between specific tags and the binary rating. For instance, certain tags might indicate the cuisine type, dietary restrictions, or meal category. These tags could influence users' preferences and impact their ratings accordingly.
The modeling algorithm chosen for this task is a Random Forest Classifier (RFC) implemented using scikit-learn's RandomForestClassifier class. The RFC algorithm is well-suited for this problem as it can handle both numerical and categorical features effectively.

The hyperparameters were tuned using a grid search with cross-validation. The best combination of hyperparameters was found, resulting in an accuracy of 93.05% on the test set. This indicates that the chosen features, 'n_ingredients' and 'tags', along with the RFC algorithm, contributed to a significantly improved model performance compared to the baseline.

## Fairness Analysis 

I run some data analysis for visualization and gain insights into the distribution of the 'binary_rating' 0s and 1s column. By creating a bar plot, it becomes easier to visualize the count of each value and observe any imbalances or patterns in the data. This information can be useful for understanding the distribution of the target variable and potentially guide further analysis or modeling decisions.
The result clearly states that the receipe has more rating {4,5}-> 1s in binary and less {0,1,2,3}-> 0s in binary. 

|    |   Count |
|---:|--------:|
|  1 |   57495 |
|  0 |    9686 |

***Visualization***
<iframe src="oneNonly/count_binary_rating.html" width=600 height=400 frameBorder=0></iframe>

***Hypothesis Testing***

Null Hypothesis: Model is fair! There is no significant association between the features 'minutes', 'n_steps', and 'n_ingredients' and the binary rating 0s and 1s.

Alternative Hypothesis: There is a significant association between the features 'minutes', 'n_steps', and 'n_ingredients' and the binary rating 0s and 1s.

_Results_

Observed value: 0.8558818713624388
P-value: 0.000999000999000999


In this case, Group X represents the combination of features 'minutes', 'n_steps', and 'n_ingredients', while Group Y represents the binary rating.

The evaluation metric used is the accuracy score, which measures the proportion of correct predictions made by the logistic regression model.

The test statistic used is the p-value, calculated through permutation testing. The model's observed score, obtained by fitting the logistic regression model with the original labels, is compared to the distribution of scores obtained by fitting the model with shuffled labels.

The significance level, also known as alpha, is not explicitly mentioned in the provided code. The p-value calculated represents the probability of observing a score as extreme as or more extreme than the observed score if the null hypothesis were true.

The resulting p-value is 0.000999000999000999, which is less than the conventional significance level of 0.05.

Conclusion: Based on the obtained p-value, which is smaller than the significance level, we reject the null hypothesis. This suggests that there is a significant association between the features 'minutes', 'n_steps', and 'n_ingredients', and the binary rating.
