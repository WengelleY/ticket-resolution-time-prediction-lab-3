## Feature Selection

The features I selected are:

- `complexity_score`
- `team_experience_years`

My target variable is:

- `resolution_hours`

## Train-Test Split

I divided the dataset into training and testing sets using an 80:20 ratio.

- Training data: 192 tickets (80%)
- Testing data: 48 tickets (20%)
- Random state: 42

I used five-fold cross-validation to compare polynomial models. Ridge and Lasso regularization strengths were also tuned using cross-validation.

Feature scaling was included in the regularized model pipelines to ensure that features measured on different scales were treated more fairly by the regularization process.


### Model Recommendation

I would choose Linear Regression for this dataset because it is simple, easy to interpret, and performs well. It achieved an MAE of approximately 2.91 hours, an RMSE of 3.57 hours, and an R² of 0.884. Although Ridge Regression performed slightly better on RMSE and R², the difference was negligible. Polynomial Regression did not improve performance. Before deployment, I would test the model on more unseen tickets, evaluate it using a time-based split if predicting future tickets, check for data leakage and missing values, and monitor prediction errors across different teams and ticket types. Collecting more representative data would also help determine whether the model generalizes reliably to real-world tickets.

## Analysis 

### 10. Why is the mean baseline useful even though it ignores all features?

The mean baseline is useful because it provides a simple reference point for evaluating more advanced models. It predicts the average resolution time calculated from the training data for every ticket, without considering any features.

### 11. Which model has the lowest MAE? Which has the lowest RMSE? Are they the same, and why might they differ?

Linear Regression achieved the lowest MAE of approximately 2.9060 hours. Ridge Regression achieved the lowest RMSE of approximately 3.5723 hours, although its result was nearly identical to Linear Regression. MAE measures the average absolute prediction error, while RMSE gives larger errors more influence because it squares the errors before averaging. Therefore, the models with the lowest MAE and RMSE can differ if their prediction errors have different distributions.

### 12. Did polynomial features improve validation performance? What is the evidence of possible overfitting?

Polynomial features did not improve performance compared with Linear Regression.

The five-fold cross-validation results were:

- Polynomial Degree 2: mean CV MAE of approximately 3.1313 hours.
- Polynomial Degree 3: mean CV MAE of approximately 3.1620 hours.

On the test set, Degree 2 achieved an MAE of approximately 3.0202 hours, while Degree 3 achieved approximately 3.1398 hours. Both performed worse than Linear Regression, which achieved an MAE of approximately 2.9060 hours. The more complex polynomial models did not improve prediction performance. This suggests that additional complexity may not be necessary and could introduce unnecessary fitting to patterns in the training data. However, these results alone do not conclusively prove overfitting.

### 13. How did regularization affect performance and coefficient magnitudes?

Regularization penalizes large coefficients to help reduce overfitting. Ridge uses an L2 penalty, which generally shrinks coefficients toward zero. Lasso uses an L1 penalty, which can shrink some coefficients all the way to zero.

### 14. Why is scaling included in the Ridge/Lasso pipeline?

Scaling puts numerical features on comparable scales before regularization. This matters because Ridge and Lasso penalize model coefficients, and features with different numerical scales can otherwise be affected unevenly. The pipeline includes feature scaling so that the scaling parameters are learned from the training data rather than from the entire dataset. During cross-validation, the scaler is fitted separately within each training fold, helping prevent data leakage.

### 15. Identify one potential leakage issue in the dataset and explain how you handled it.

The `reopened` feature is a potential source of data leakage. If the goal is to predict resolution time before a ticket is resolved, whether that ticket is reopened may only become known after the initial resolution. Using this feature could provide information that would not be available at prediction time. Therefore, `reopened` was excluded from the selected features.


### 16. Would a random split be appropriate if tickets were collected over several years or came from repeated customers/teams? Explain.

no. If tickets were collected over several years and the goal is to predict future resolution times, a time-based split would be more realistic. Older tickets would be used for training, and newer tickets would be used for testing. This would help evaluate how the model performs on future data. If the dataset contains repeated customers or teams, a group-based split may be more suitable. Tickets from the same customer or team would be kept together in either the training or testing set. This helps evaluate whether the model generalizes to customers or teams it has not seen before.

### 17. What additional tests would you perform before deploying this model?


Test on new data:



Check data quality:



Check for leakage:



Evaluate across different groups:



Test on future tickets:
