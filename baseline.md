---
layout: default
title: Baseline Model
nav_order: 6
---
# Baseline Model

For our baseline model, we performed **linear regression** using a basic set of engineered and original features from the `recipes` dataset. This is a naive first approach to our prediction task: estimating a recipe’s average rating based only on Food.com metadata.

We used all the features we mentioned in the previous step, with their appropriate justifications as well.

All features were numerical or transformed into numerical values, so no encoding of nominal (categorical) variables was required at this stage. If we had included a feature like `tag` or `cuisine`, those would require additional encoding such as one-hot or target encoding.

To train and evaluate our model properly, we built a complete scikit-learn `Pipeline`, which includes:

1. A preprocessing step that converts submission dates into numeric values.
2. A linear regression model that fits the transformed features to the average rating.

We split our dataset into training and test sets using an 80/20 split and evaluated the model on the test set. The evaluation metric we used was **Root Mean Squared Error (RMSE)**, because it provides a sense of how far off, on average, our predicted ratings are from the true ratings. This helps us have a train/test split that doesn't overfit too much (generalizes well) while still having a low RMSE.

While the model is not expected to perform exceptionally well, it provides a useful lower bound for what performance looks like when using only raw numerical features. This model is **not "good"** in the sense of high predictive accuracy, but it’s a reasonable starting point.

```python
train_rmse = mean_squared_error(pl.predict(X_train),y_train)
test_rmse = mean_squared_error(pl.predict(X_test), y_test)
print(train_rmse)
print(test_rmse)
print(f"RMSE of train is {100 * (1-(train_rmse)/(test_rmse))}% less than test")
```
```
0.28596046381271356
0.4421927551340672
RMSE of train is 35.331264365465685% less than test
```
As you can see the mean-squared error overfits by about 35% of the RMSE. We will also look at the residual plot to observe whether or not a linear model is appropriate.
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/baseline_resid.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>
It seems like through linear regression, the residuals are still heteroskedastic, even though they follow a linear pattern. There is also a lot of density in the 4 and 5 which suggests that our model tends to predict a 4 or 5 without sacrificing a lot of MSE loss. We want to perform a correction on this heteroskedasticity in our [Final Model](https://kenjigunawan.github.io/gasorpass/finalmodel.html).