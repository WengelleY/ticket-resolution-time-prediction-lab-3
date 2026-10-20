

The  features I selected are:



\- `complexity\_score`

\- `team\_experience\_years`



my target variable is:



\- `resolution\_hours`




\##  Train-Test Split



i divided dataset  into training and testing sets using an 80:20 ratio.



\- Training data: 192 tickets (80%)

\- Testing data: 48 tickets (20%)

\- Random state: 42




and used Five-fold cross-validation  compare polynomial models, Ridge and Lasso regularization strengths were also tuned using cross-validation.



Feature scaling was included in the regularized model pipelines to ensure that features measured on different scales were treated more fairly by the regularization process.





\##  Model Evaluation Results



The following results were obtained on the test set of 48 tickets.



| Model | MAE (hours) | RMSE (hours) | R² |

|---|---:|---:|---:|

| Mean Baseline | 9.1140 | 10.5977 | -0.0210 |

| Linear Regression | 2.9060 | 3.5723 | 0.8840 |

| Polynomial Regression (Degree 2) | 3.0202 | 3.7409 | 0.8728 |

| Polynomial Regression (Degree 3) | 3.1398 | 3.9489 | 0.8582 |

| Ridge Regression | 2.9060 | 3.5723 | 0.8840 |

| Lasso Regression | 2.9071 | 3.5724 | 0.8840 |



this  shows that Linear Regression achieved the lowest MAE and RMSE among the tested models, although Ridge Regression produced nearly identical results.



\### Cross-Validation Results



The polynomial models were compared using five-fold cross-validation.



| Model | Mean CV MAE (hours) | Standard Deviation |

|---|---:|---:|

| Polynomial Degree 2 | 3.1313 | 0.3097 |

| Polynomial Degree 3 | 3.1620 | 0.1882 |



Degree 2 achieved a slightly lower mean cross-validation MAE than Degree 3. However, neither polynomial model improved upon the test performance of Linear Regression.



\## 8. Linear Regression Equation



The fitted Linear Regression model produced the following approximate equation:



\\\[

\\begin{aligned}

\\text{Predicted Hours} ={}\& 4.2493\\\\

\&+ 3.0799(\\text{Complexity Score})\\\\

\&- 0.3181(\\text{Team Experience Years})

\\end{aligned}

\\]



### Model Recommendation

I would choose **Linear Regression** for this dataset because it is simple, easy to interpret, and performs well. It achieved an MAE of approximately 2.91 hours, an RMSE of 3.57 hours, and an R² of 0.884. Although Ridge Regression performed slightly better on RMSE and R², the difference was negligible. Polynomial Regression did not improve performance.

Before deployment, I would test the model on more unseen tickets, evaluate it using a time-based split if predicting future tickets, check for data leakage and missing values, and monitor prediction errors across different teams and ticket types. Collecting more representative data would also help determine whether the model generalizes reliably to real-world tickets.



## 10. Analysis Questions and Answers

### 10. Why is the mean baseline useful even though it ignores all features?

The mean baseline is useful because it provides a simple reference point for evaluating more advanced models. It predicts the average resolution time calculated from the training data for every ticket, without considering any features.


### 11. Which model has the lowest MAE? Which has the lowest RMSE? Are they the same, and why might they differ?

Linear Regression achieved the lowest MAE of approximately 2.9060 hours. Ridge Regression achieved the lowest RMSE of approximately 3.5723 hours, although its result was nearly identical to Linear Regression.

MAE measures the average absolute prediction error, while RMSE gives larger errors more influence because it squares the errors before averaging. Therefore, the models with the lowest MAE and RMSE can differ if their prediction errors have different distributions.

In this experiment, the differences between Linear Regression and Ridge Regression are extremely small.

### 12. Did polynomial features improve validation performance? What is the evidence of possible overfitting?

Polynomial features did not improve performance compared with Linear Regression.

The five-fold cross-validation results were:

- Polynomial Degree 2: mean CV MAE of approximately 3.1313 hours.
- Polynomial Degree 3: mean CV MAE of approximately 3.1620 hours.

On the test set, Degree 2 achieved an MAE of approximately 3.0202 hours, while Degree 3 achieved approximately 3.1398 hours. Both performed worse than Linear Regression, which achieved an MAE of approximately 2.9060 hours.

The more complex polynomial models did not improve prediction performance. This suggests that additional complexity may not be necessary and could introduce unnecessary fitting to patterns in the training data. However, these results alone do not conclusively prove overfitting.

### 13. How did regularization affect performance and coefficient magnitudes?

Ridge and Lasso Regression produced results nearly identical to Linear Regression.

- Linear Regression: MAE approximately 2.9060 hours.
- Ridge Regression: MAE approximately 2.9060 hours.
- Lasso Regression: MAE approximately 2.9071 hours.

Regularization penalizes large coefficients to help reduce overfitting. Ridge uses an L2 penalty, which generally shrinks coefficients toward zero. Lasso uses an L1 penalty, which can shrink some coefficients all the way to zero.


### 14. Why is scaling included in the Ridge/Lasso pipeline?

Scaling puts numerical features on comparable scales before regularization. This matters because Ridge and Lasso penalize model coefficients, and features with different numerical scales can otherwise be affected unevenly. The pipeline includes feature scaling so that the scaling parameters are learned from the training data rather than from the entire dataset. During cross-validation, the scaler is fitted separately within each training fold, helping prevent data leakage.

### 15. Identify one potential leakage issue in the dataset and explain how you handled it.

The `reopened` feature is a potential source of data leakage. If the goal is to predict resolution time before a ticket is resolved, whether that ticket is reopened may only become known after the initial resolution.

Using this feature could provide information that would not be available at prediction time. Therefore, `reopened` was excluded from the selected features.

The `lines_changed` feature was also excluded because the number of lines changed may not be known before development work begins.

### 16. Would a random split be appropriate if tickets were collected over several years or came from repeated customers/teams? Explain.

A random split may not be appropriate in these situations.

If tickets were collected over several years and the goal is to predict future resolution times, a time-based split would be more realistic. Older tickets would be used for training, and newer tickets would be used for testing. This would help evaluate how the model performs on future data.

If the dataset contains repeated customers or teams, a group-based split may be more suitable. Tickets from the same customer or team would be kept together in either the training or testing set. This helps evaluate whether the model generalizes to customers or teams it has not seen before.


### 17. What additional tests would you perform before deploying this model?

Before deploying the model, I would perform the following tests:

1. **Test on new data:*
2. **Check data quality:*
3. **Check for leakage:*
4. **Evaluate across different groups*
5. **Test on future tickets*

