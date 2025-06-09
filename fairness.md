---
layout: default
title: Fairness Analysis
nav_order: 8
---
# Fairness Analysis

To evaluate whether our model performs equally well across different ranges of average recipe ratings, we conducted a fairness analysis comparing high-rated recipes (with average ratings between 4 and 5) and low-rated recipes (with ratings between 1 and 3). This distinction is meaningful because the dataset is heavily skewed toward higher ratings, making it important to examine whether this imbalance affects the model’s predictive accuracy across groups.

We hypothesized that the model **performs better** on **high-rated** recipes than on *low-rated* ones. This hypothesis is supported visually by the residual plot, which shows that predicted ratings mostly fall between 3.5 and 4.5. This suggests the model may be systematically underfitting low-rated entries and compressing predictions toward the high end. If this is true, the model could be less accurate when predicting low-rated recipes, raising concerns about bias or reduced utility for certain subsets of data.

Therefore here are the hypotheses we are going to test for fairness:

* Null Hypothesis: The model performs equally well on both high-rated (4–5) and low-rated (1–3) recipes. Any observed difference in prediction error distributions between the two groups is due to random chance.

* Alternative Hypothesis: The model performs better on high-rated (4–5) recipes than on low-rated (1–3) recipes. Specifically, the prediction errors for low-rated recipes are systematically larger than those for high-rated recipes.

To test this, we performed a permutation test using the Kolmogorov–Smirnov (KS) statistic on the absolute prediction errors of both groups. The test assessed whether the distribution of residuals differs significantly between high-rated and low-rated recipes.

```
Observed KS statistic = 0.9969
```

This is almost the maximum of 1. After the Monte Carlo simulation with 5000 iterations, we obtained the following.
<iframe
  src="https://kenjigunawan.github.io/gasorpass/assets/html/fairness_ks.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

Since our permutation test returned a p-value of approximately 0, we have strong evidence to **reject the null hypothesis**. This result indicates that the observed difference in model performance between high-rated (4–5) and low-rated (1–3) recipes is **not likely** due to random chance. Specifically, the model performs **significantly better** at predicting higher ratings than lower ones.

This suggests that our model is **not fair** across the range of ratings. Any conclusions drawn from the model may disproportionately reflect the patterns of **high-rated recipes**, and *caution* should be used when interpreting predictions for *low-rated ones*.