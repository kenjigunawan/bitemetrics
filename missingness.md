---
layout: default
title: Assessment of Missingness
nav_order: 3
---

# Assessment of Missingness

First, the review is not that much use in this data because:
1. It's not quantitative, and
2. It's not categorical: doesn't contain enough commonalities across different recipes.

Next, we changed the ratings from 0 to NaN because ratings need to be at least 1-star. When you input a rating, you are suggesting that you've tried the recipe out and have tasted the recipe, and judged its cooking time so you would feel more qualified to rate the recipe. If you didn't do these things, your ratings wouldn't be as useful, so you would be less-inclined to do so (even if some people still do it nonetheless).

Now we're going to look at which columns contain missing values. We found that 3 columns contain missing values after the preprocessing above, i.e. `rating`, `name`, and `description`.

There are `51832` rows which are missing ratings! Whereas only 1 row is missing in `name` and 70 missing in `description`.

Reading the reviews suggests that people do not rate for several reasons.
1. They are just adding a comment for the recipe.
    A user who loved the dish may be more inclined to rate it than one who thought it was mediocre. So the decision to skip a rating is plausibly related to the (unobserved) true rating.
2. They changed the recipe in some way, so they feel like their result wouldn't be a fair rating.
    People often tweak when they expect a better (sometimes worse) outcome. That expectation again ties back to the rating they would have given.
3. They are talking about something that's irrelevant to the recipe.
    Even if the comment is off-topic, choosing not to rate might reflect indifference about the dish’s quality, which is again, correlated with the unreported rating.
4. They didn't feel like they did the recipe properly but they still wanted to comment nonetheless.
    Likely tied to disappointment or uncertainty which is information about the missing rating itself.

In all four cases the propensity to omit a rating probably depends on the value of the rating that would have been given. Therefore, even with the review column, the rating column is **NMAR (Not Missing At Random)**.

For 'name' only one row has no name, and looking at the steps it looks like the steps are very concise. It seems like a draft recipe that accidentally makes it into the csv. Hence, it is only interesting to inspect whether 'description' is MCAR, MAR, or NMAR.

## Is `description` Missing by Design, MCAR, MAR or NMAR?

It's clearly not missing by design because description contains additional words that are not just the tags. It's a human-readable *description* of the recipe, unlike tags. Even though the content might be correlated with the tags, but we can't recreate a missing description.

To answer this question, we will assess whether it's plausibly NMAR before proceeding to the test for MAR or MCAR.

```
Index(['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags',
       'nutrition', 'n_steps', 'steps', 'description', 'ingredients',
       'n_ingredients'],
      dtype='object')
```

Looking through the columns, it doesn't seem like the columns have any correlation with a missing description, especially the fact that other columns are either categorical, numeric, or are identifiers for that particular recipe (like `name`, `id`, `contributor_id`). So let's figure out whether or not `description` is Missing by Design, NMAR, MAR, or MCAR.

Is there a good reason why the missingness of the description depends on the values themselves? Out of the 83782 recipes, only 70 are missing.

We'll reason about the data generation process. Let's take a look at an example description. The first row's description is as follows:

> "These are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....seriously! There's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!"

They don't seem like they are generated from any other thing except the recipe writers' creativity (and probably include words from the tag), but so far **nothing suggests that the data might be NMAR**. So we'll just perform a **MAR/MCAR** test.

### The MAR/MCAR Test: Permuted K-S Test

The Permuted Kolmogorov-Smirnov Test consists of two things:
1. The test statistic is the **Kolmogorov-Smirnov** Test Statistic, which is the **maximum absolute difference** of the values of the Cumulative Distribution Functions of the two samples.
2. We perform repeated **permutation tests** to simulate the null hypothesis of the population coming from the same distribution, and this generation process will provide us with an approximate p-value (by the Law of Large Numbers and Central Limit Theorem for approx. rate of convergence).

Without further ado, here is one run of the test result (with 1000 iterations).
```
minutes      observed KS = 0.0833,  p-value = 0.563
n_steps      observed KS = 0.1338,  p-value = 0.074
n_ingredients observed KS = 0.1760,  p-value = 0.013
```
It seems like via this permuted K-S test we had in lecture, with a 0.05 significance level, the description is MAR with dependence on the `n_ingredients`, which is interesting. Why can this be?

Let's look at the average `n_ingredients` of both classes.
```python
print(raw_recipes[raw_recipes['description'].isna()]['n_ingredients'].mean())
print(raw_recipes['n_ingredients'].mean())
```
```
7.742857142857143
9.214019717839154
```
There are on average, way fewer ingredients used for recipes with no description, and this might be due to some sort of mishandling of recipes which are simpler to make, or are unfinished recipes (they are still being tested, so they don't have a description yet)! Nonetheless, being conditional on this means it's **Missing at Random** conditioned on number of ingredients.

We do not perform a TVD test on the categorical columns because each categorical column contains more than 1 value, so because we only have 70 missing data points, the curse of dimensionality kicks in.