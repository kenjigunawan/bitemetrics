---
layout: default
title: Framing a Prediction Problem
nav_order: 5
---
# Framing a Prediction Problem

We aim to **predict the average rating** a recipe receives based on all sensible variables available in the `recipes` dataset. This is a **regression problem**, as the response variable, average rating, is continuous and typically ranges between 1.0 and 5.0.

First, we inspect the datatypes in `recipes` to determine which columns are **numerical**, **categorical**, or simply **identifiers**. We discard identifiers such as `'id'` and `'contributor_id'`, which cannot be determined before the recipe is published. We retain features such as `'minutes'`, `'n_ingredients'`, and the nutrition-related variables (`'calories'`, `'sugar'`, `'protein'`, etc.) since they plausibly affect a cook’s rating.

The **response variable** is the `avg_rating` of each recipe, computed from user-submitted ratings in the `interactions` table. We chose this variable because it's a strong indicator of perceived recipe quality and is relevant for use cases like recipe recommendation or search ranking, hence this is a **Regression** problem.

We chose **Root Mean Squared Error (RMSE)** ($L^2$-norm) as our primary evaluation metric because it provides an interpretable measure of the typical prediction error in units of the response variable (i.e., star rating). RMSE penalizes larger errors more heavily than Mean Absolute Error (MAE), which is desirable in our setting where a prediction that is off by 2 stars is worse than one that is off by 1 star.

Importantly, we ensure that our features reflect only information available **at the time the recipe is published**, and because of our hypothesis test; can also predict its rating over time (although it is going to decrease). For instance, we do not include any reviews as input features, since those are generated after the recipe is live. This ensures that our model makes fair predictions without leaking future information (prevent look-ahead bias).

We select the following categories:

1. `minutes`: How long it takes to prepare the food.  
2. `carbohydrates`: The amount of carbohydrates in the recipe.  
3. `submitted`: The number of days the recipe has been submitted into the website.  
4. `n_steps`: Number of steps in the recipe instructions.  
5. `n_ingredients`: Number of ingredients used in the recipe.  
6. `calories`: The total caloric content of the recipe.  
7. `total_fat`: The amount of total fat in the recipe.  
8. `sugar`: The amount of sugar in the recipe.  
9. `sodium`: The amount of sodium in the recipe.  
10. `protein`: The amount of protein in the recipe.  
11. `saturated_fat`: The amount of saturated fat in the recipe.  

These are each sensible predictors for the average rating given. It's important to note that `submitted` is also included, despite not being available at the time the recipe is published because our Hypothesis Test showed strong evidence that it plays a significant role in predicting the average rating a recipe will receive, with a very low p-value.

Also while predicting, if you want to see the average rating of the recipe at the time of publishing, you can impute `submitted` to be 0.

Originally our `submitted` column was the date of submission, but now it has been changed into the number of days it has been out before the recipe gets rated.
```
minutes             int64
submitted           int64
n_steps             int64
n_ingredients       int64
calories          float64
total_fat         float64
sugar             float64
sodium            float64
protein           float64
saturated_fat     float64
carbohydrates     float64
```

<img src="https://kenjigunawan.github.io/gasorpass/assets/images/gas%20or%20pass%20logo%20only.svg" alt="Gas or Pass Logo" style="float: right; margin: 0 0 1em 1em; width: 200px;" />
Now every column is of a numerical data type, we can perform our **Regression** of the average rating. Let's get cooking!