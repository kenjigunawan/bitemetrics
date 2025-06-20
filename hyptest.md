---
layout: default
title: Hypothesis Testing
nav_order: 4
---

# Hypothesis Test: Does Newness of Interaction Affect Rating?

As mentioned in our earlier bivariate analysis, we were curious whether the **newness** of an interaction influences how it is rated. Specifically, we wanted to know if recipes that receive high ratings (4 or 5) tend to be **newer** than those that receive low ratings (1–3). Here, *newness* refers to the difference in days between the date the recipe was rated and the date it was first submitted.

To investigate this question, we conducted a **permutation test** with the following setup:

* **Null Hypothesis**: The newness of interactions comes from the same distribution regardless of the rating given.
* **Alternative Hypothesis**: The newness of interactions rated highly (4 or 5) differs from those rated low (1–3).
* **Test Statistic**: The mean of newness in the high-rating group minus the mean of newness in the low-rating group.
* **Significance Level**: 0.05

We used a permutation test because we don’t assume any specific population distribution and want to evaluate whether the observed difference in newness could plausibly arise under the null hypothesis. The **directional hypothesis** stems from the idea of **selection bias**: newly posted recipes reflect a more representative spread of ratings, whereas older recipes may be rated disproportionately by users who had a negative experience.

To perform the test:
1. We split the data into two groups:
   * **High-rating** (ratings of 4 or 5)
   * **Low-rating** (ratings of 1–3)
2. We computed the observed test statistic.
3. We shuffled the ratings **5000 times** and computed the test statistic for each shuffled dataset to generate a null distribution.
4. We then calculated the **p-value** as the proportion of shuffled test statistics more extreme than the observed value.

## Result
* **Observed statistic**: -312.19
* **p-value**: Basically 0
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/hyptest.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Thus, with any usual significance level (e.g., α = 0.01 or α = 0.05), we **reject the null**.

The newness of interactions rated highly (4 or 5) **does** come from a different distribution than those rated low (1–3). This supports the hypothesis of **selection bias**, that is, newer recipes tend to receive more balanced feedback, while older ones may attract ratings from users more motivated to leave negative reviews.

A plausible reason might be as follows: when a recipe is newly released, nobody has tried the recipe so it represents a more accurate view of how good a particular recipe is. Whereas, in older recipes; good recipes are less inclined to rate, whereas if the recipe is bad the audience would be just as happy to point it out.

However, using the test above we can't *conclude* the above cause, it's just one possible explanation.