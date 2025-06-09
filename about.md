---
layout: default
title: Data Cleaning and Exploratory Data Analysis
nav_order: 2
---

# Data Cleaning and Exploratory Data Analysis

We first perform a merging of the recipes and interactions DataFrame as we want to incorporate recipes with all its comments/interactions. This is just so that we can work with everything on the same DataFrame.

<iframe 
  src="https://kenjigunawan.github.io/gasorpass/assets/html/mergeddf.html" 
  width="800" 
  height="600" 
  style="border:none; margin: 20px 0;"
></iframe>

We also add the parameter "average ratings" for each of the recipes we mentioned. Furthermore, we one-hot encode the ratings and stick it to the original "recipes" DataFrame.

Next, we parse the nutrition (i.e. we explode the DataFrame for nutrition) in order to be able to access the information properly.

Last, we combine these parsed information into the interactions DataFrame. So now we can perform proper analysis and prediction on either the merged DataFrame which includes every interaction and recipe which has an interaction (since we joined 'inner'), or the interaction DataFrame, or the recipe DataFrame by itself.

We have finished cleaning. The DataFrame contains `(219393, 23)` rows and columns. The datatype of each column is shown as follows.

| `col_name`        | `dtype`   |
|-------------------|-----------|
| `user_id`         | int64     |
| `recipe_id`       | int64     |
| `date`            | object    |
| `rating`          | float64   |
| `review`          | object    |
| `name`            | object    |
| `id`              | int64     |
| `minutes`         | int64     |
| `contributor_id`  | int64     |
| `submitted`       | object    |
| `tags`            | object    |
| `n_steps`         | int64     |
| `steps`           | object    |
| `description`     | object    |
| `ingredients`     | object    |
| `n_ingredients`   | int64     |
| `calories`        | float64   |
| `total_fat`       | float64   |
| `sugar`           | float64   |
| `sodium`          | float64   |
| `protein`         | float64   |
| `saturated_fat`   | float64   |
| `carbohydrates`   | float64   |

The first 5 rows of our cleaned DataFrame is as follows.

<iframe 
  src="https://kenjigunawan.github.io/gasorpass/assets/html/cleaneddf.html" 
  width="100%" 
  height="600" 
  style="border:none; margin: 20px 0;"
></iframe>

## Univariate Analysis

First, we want to see how the newness of a recipe relates to the fact that the recipes get rated or not. To do this, we want to look at the probability density (histogram) of the relationship between the newness of a recipe and wheher or not it gets rated.

<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/univar_analysis.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

This demonstrates newer recipes are rated more often. Furthermore, it looks like it follows some sort of power rule for the distribution.

If you recall the "Pareto Principle", it appears everywhere, even in this dataset! This suggests that the data is mostly properly collected. As for the ones with negative 'newness', these are outliers because of how they deal with the 'submitted' date, or some other errors, but looking at the graph it suggests that this phenomenon doesn't happen very often, so we can safely ignore it.

## Bivariate Analysis

For this section, we are going to investigate whether newness relates with the rating of a recipe, and what rating that they get; so the two variables are rating given (which directly corresponds to *whether they rate in the first place*) with the newness of the recipe.
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/bivar_analysis.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

As you can see, the proportion of ratings in the newer recipes (less than 500 days) is somewhat similar, but then the trend shifts to lower ratngs (in particular 1's and 2's) for older recipes. It means most of the good ratings are given close to the time of release, and worse ratings are given far after the time of release.

This might be due to selection bias. In particular, when a recipe is newly released, nobody has created the recipe so it represents a more accurate view of how good a particular recipe is, because the audience are motivated chefs or amateur cooks. Whereas, in older recipes; they get *stale* (pun intended), if the recipe is good the audience who would read that are not particularly invested with the website so are less inclined to rate, whereas if the recipe is bad they would be just as happy to point it out. However, this needs to be tested via a [hypothesis test](https://kenjigunawan.github.io/gasorpass/hyptest.html).
