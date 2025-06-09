---
layout: default
title: Final Model
nav_order: 7
---
# Final Model
First, it is sensible to think that our model has a a lot of multicollinearity which might have contributed to the high error - they fail to converge quickly, and so we performed a column-by-column correlation test to see which ones are multicollinear.
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/corrdf.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

During feature exploration, we observed that several variables exhibited multicollinearity. Most notably, calories with both `total_fat` and `carbohydrates`, and `total_fat` with `saturated_fat`. This redundancy suggested that some form of feature selection would be beneficial for reducing overfitting and improving model interpretability.

We also experimented with non-linear transformations, including power-based transformations and non-linear fitting techniques. However, these approaches did not lead to any meaningful performance gains on our validation data. As a result, we chose not to incorporate them into our final model.

As a result, in order to perform feature selection, we did 2 things:
1. We just normalized every variable, because we wanted to perform *regularized regression*.
2. We added **regularization** (penalty) on our model, i.e. `Lasso` (penalizes `L1`-norm) to perform feature selection for us.

We then constructed a full pipeline using `sklearn.Pipeline` that encapsulates all preprocessing and modeling steps. The numeric features were first standardized and then quantile-transformed. The categorical features were one-hot encoded and scaled (to prevent errors if we added categorical columns).

We then fit a `Lasso` regression model with a fixed regularization parameter `alpha=0.01`. This value was selected based on a few experimentation; `alpha=0.1` was tried to be too high because it set almost all coefficients to zero, while `alpha=0.001` was too slow to converge and had too high of a variance.

The use of `Lasso` allowed us to perform automatic **feature selection** by shrinking some coefficients to zero, particularly among collinear or less informative features. This not only simplified the model but also **reduced the risk of overfitting**.

Using the same train-test split as the baseline model, we evaluated our final model using RMSE.
```python
final_pipeline.fit(X_train, y_train)
print(mean_squared_error(final_pipeline.predict(X_train), y_train)), print(mean_squared_error(final_pipeline.predict(X_test), y_test))
```
```
0.39330204133817576, 0.4022151508851836
```
The final model achieved a lower test RMSE about 10%, indicating that the predictions were, on average, closer to the true average ratings than those from the baseline model. Also, the test and train RMSEs are very close to each other, suggesting a more generalizable model.
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/lasso_resid.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

Finally, examining the residual plot and the fitted OLS line of the residuals, we observe that the line has a slope close to zero. This suggests a balanced distribution of residuals above and below the predicted values, indicating that the model does not systematically over- or under-predict, suggesting *unbiasedness* of the estimator.

Additionally, the similar RMSE values on the training and testing splits suggest that the model generalizes well and is not overfitting. Together, these observations indicate that this is likely the **best linear model** we can achieve with the current set of features.